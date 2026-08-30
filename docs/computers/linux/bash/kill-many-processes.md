# Terminate Optimizer Worker Processes

This document describes a targeted script for terminating optimizer worker
processes. It avoids killing unrelated processes that merely contain the word
`hyper` in their command line.

## Important safety note

Stop the active optimizer sweep in the dashboard before running the script.
Otherwise, the sweep manager may immediately create replacement workers.

The script sends `SIGTERM` first, waits briefly, and then sends `SIGKILL` to
remaining matching processes. The second signal also removes workers that are
stuck in the stopped (`T`) process state.

Completed optimizer results remain persisted on disk and can be resumed after
the dashboard is restarted.

## Script

Save the following as `kill_optimizer_workers.sh`:

```bash
#!/usr/bin/env bash

# Terminate Python module processes matching PROCESS_SUBJECT.
# The first positional argument takes precedence over the environment variable.
# Example:
#   ./kill_optimizer_workers.sh 'optimizer\.hyperopt_runner'
#   PROCESS_SUBJECT='optimizer\.hyperopt_runner' ./kill_optimizer_workers.sh

set -u

PROCESS_SUBJECT="${1:-${PROCESS_SUBJECT:-optimizer\.hyperopt_runner}}"
PYTHON_EXEC_RE='(^|/)python(3)?$'

optimizer_pids() {
  ps -eo pid=,args= |
    awk -v subject="$PROCESS_SUBJECT" -v python_exec="$PYTHON_EXEC_RE" '
      $2 ~ python_exec &&
      $0 ~ ("(^|[[:space:]])-m[[:space:]]+" subject "([[:space:]]|$)") {
        print $1
      }
    '
}

terminate_processes() {
  local signal="$1"
  local pids

  pids="$(optimizer_pids)"

  if [[ -z "$pids" ]]; then
    echo "No matching optimizer processes found."
    return
  fi

  echo "Sending SIG${signal} to:"
  echo "$pids"

  # PIDs come directly from ps and are numeric.
  kill "-${signal}" $pids 2>/dev/null || true
}

echo "Matching processes before termination:"
optimizer_pids

terminate_processes TERM

# Give normal shutdown a moment to complete.
sleep 2

# Remove workers that did not respond, including stopped (T-state) workers.
terminate_processes KILL

echo
if [[ -z "$(optimizer_pids)" ]]; then
  echo "All matching processes have been removed."
else
  echo "Some matching processes remain:"
  optimizer_pids
fi
```

## Usage

Make the script executable:

```bash
chmod +x kill_optimizer_workers.sh
```

Terminate the optimizer workers:

```bash
./kill_optimizer_workers.sh
```

The default target is:

```text
optimizer.hyperopt_runner
```

To pass a different module without editing the script:

```bash
./kill_optimizer_workers.sh 'another\.module_name'
```

The same value can be supplied through the environment:

```bash
PROCESS_SUBJECT='another\.module_name' ./kill_optimizer_workers.sh
```

The positional argument takes precedence over the environment variable.

## Why `ps -aux | grep hyper | wc -l` is unreliable

That command also counts the `grep` process itself and any unrelated command
containing `hyper`. The script matches the exact Python module invocation:

```text
python -m optimizer.hyperopt_runner
```

It therefore does not terminate unrelated dashboard, exchange, or test
processes.

## After termination

Restart the dashboard so the current optimizer concurrency settings are
loaded. Then resume the persisted sweep through the dashboard or the supported
`/api/optimizer/sweep/resume` endpoint.
