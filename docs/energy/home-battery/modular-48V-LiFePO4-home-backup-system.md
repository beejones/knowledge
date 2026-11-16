Excellent. I would build this as a **modular 48V LiFePO4 home backup system**, inspired by Will Prowse's design, but adapted to European electrical standards and Belgian regulations.

The idea is to start with approximately 10kWh of battery storage, have automatic switching when the electricity fails, and make it possible to expand to 20–30kWh without replacing the original equipment.

I would also prepare the installation for solar panels at a later stage, even though we won't install them initially.

# 1. Proposed system architecture

My recommendation is to use components from Victron Energy and Pylontech.

Why?

- Modular architecture.
- Good compatibility between inverter and batteries.
- Excellent monitoring capabilities.
- Expandable battery capacity.
- European 230V/50Hz equipment.
- Established ecosystem and technical documentation.

Here is the architecture I propose:



### Electrical architecture

```text
                  BELGIAN ELECTRICITY GRID
                            |
                            |
                      Main Distribution
                            |
                            |
                     AC INPUT PROTECTION
                            |
                            |
                   +--------------------+
                   |                    |
                   |   VICTRON          |
                   |   MULTIPLUS-II     |
                   |   48V / 5000VA     |
                   |                    |
                   +---------+----------+
                             |
                             |
                       AC OUTPUT
                             |
                             |
                    BACKUP DISTRIBUTION
                             |
                  +----------+----------+
                  |          |          |
                  |          |          |
                Lights    Fridge      Router
                  |          |          |
                  +----------+----------+
                             |
                             |
                      48V DC BATTERY
                             |
                     DC FUSE / ISOLATOR
                             |
                   +---------+---------+
                   |                   |
              PYLONTECH 1         PYLONTECH 2
                4.8kWh              4.8kWh
                   |                   |
                   +---------+---------+
                             |
                       BATTERY BUSBAR

```

The important feature is that the inverter acts as both:

1. An inverter, converting battery DC into 230V AC.
2. A battery charger, charging the batteries from the electricity grid.

It also contains an automatic transfer switch.

When the grid fails, the inverter switches to battery operation.

When the grid returns, it switches back and can recharge the batteries.

This is precisely what we want.

---

# 2. Battery storage: approximately 10kWh

I recommend starting with two Pylontech US5000 batteries.



## Pylontech US5000

Technical specifications:

| Specification | Value |
|---|---:|
| Battery chemistry | LiFePO4 |
| Nominal voltage | 48V |
| Capacity | 100Ah |
| Energy | 4.8kWh |
| Usable energy | 4.56kWh |
| Continuous charge/discharge | 100A |
| Maximum discharge | 120A for 15 minutes |
| Weight | 39.7kg |
| Communication | CAN / RS485 |
| Cycle life | >8,000 cycles |

Source: Pylontech manufacturer specifications. [en.pylontech.com.cn](https://en.pylontech.com.cn/products/c23/134.html?utm_source=chatgpt.com)

Two batteries provide:

| Configuration | Capacity |
|---|---:|
| Battery 1 | 4.8kWh |
| Battery 2 | 4.8kWh |
| Total nominal | 9.6kWh |
| Total usable | 9.12kWh |

This is a good starting point.

### Future expansion

One advantage of this system is that you can add more batteries.

| Number of batteries | Nominal capacity | Usable capacity |
|---|---:|---:|
| 2 | 9.6kWh | 9.12kWh |
| 3 | 14.4kWh | 13.68kWh |
| 4 | 19.2kWh | 18.24kWh |
| 5 | 24kWh | 22.8kWh |
| 6 | 28.8kWh | 27.36kWh |

Pylontech specifies support for up to 16 modules in a single string, subject to the installation requirements.

My suggestion is to install a battery rack that can accommodate at least six batteries from the beginning.

That avoids rebuilding the installation later.

---

# 3. Inverter: Victron MultiPlus-II



This is the central component.

I recommend:

**Victron MultiPlus-II 48/5000/70-50 230V**

Specifications:

| Specification | Value |
|---|---:|
| Battery voltage | 48V |
| AC output | 230V |
| Frequency | 50Hz |
| Apparent power | 5000VA |
| Continuous active power | 4000W |
| Peak power | 9000W |
| Battery charging current | 70A |
| Automatic transfer switch | 50A |
| Maximum efficiency | 96% |

Source: Victron technical documentation. [Victron Energy](https://www.victronenergy.com/media/pg/MultiPlus-II_230V/en/technical-specifications-mp-ii-230v.html?utm_source=chatgpt.com)

Notice something important:

Although it is advertised as a 5000VA inverter, its continuous real power output is 4000W at 25°C.

That is still sufficient for many essential household appliances.

### Why this inverter?

It provides:

- Automatic switching during a power failure.
- Battery charging from the electricity grid.
- Pure sine-wave output.
- Configurable charging parameters.
- Remote monitoring.
- Possibility of adding solar charging equipment later.

It also supports parallel operation and more advanced system configurations.

One important Belgian consideration: the exact model, firmware and grid-connected configuration must be checked against the applicable Synergrid requirements before purchase.

---

# 4. Monitoring: Victron Cerbo GX



I would add a Cerbo GX.

This allows you to monitor the complete installation.

You can see:

- Battery state of charge.
- Remaining battery capacity.
- Battery voltage.
- Charging current.
- Inverter power consumption.
- AC input power.
- AC output power.
- System alarms.

You can access the information through Victron's VRM monitoring portal.

This is particularly useful when you are away from home.

You could, for example, check whether your battery is fully charged before a forecasted storm.

The Cerbo GX also provides integration possibilities with Home Assistant.

Given your existing Home Assistant and Node-RED environment, this is something I would definitely implement.

---

# 5. Battery rack and electrical protection

I would not mount the components on a hand truck for a permanent home installation.

Instead, I would use a metal battery rack.



Recommended configuration:

- Six-battery-capacity rack.
- Two batteries initially.
- Room for four additional batteries.
- Inverter mounted separately.
- Proper ventilation.
- Protected DC distribution.
- Accessible emergency isolation.

### Protection components

You will need:

| Component | Purpose |
|---|---|
| DC battery fuse | Protects battery cables |
| DC isolator | Disconnects battery system |
| DC busbars | Distributes battery current |
| Battery communication cables | BMS communication |
| AC input breaker | Protects grid input |
| AC output protection | Protects backup circuits |
| RCD protection | Residual-current protection |
| Earthing system | Electrical safety |
| Enclosure | Protects electrical components |

The exact fuse ratings, cable cross-sections, earthing arrangement and RCD configuration must be calculated for the final installation.

I would have a qualified Belgian electrician design and commission the electrical connections.

---

# 6. How much backup electricity will you have?

Let's assume you have 9.12kWh of usable battery energy.

Allowing approximately 10% conversion losses, you have around 8.2kWh available to your appliances.

Some examples:

| Average consumption | Approximate backup time |
|---|---:|
| 200W | 41 hours |
| 300W | 27 hours |
| 500W | 16 hours |
| 750W | 11 hours |
| 1000W | 8 hours |
| 1500W | 5.5 hours |
| 2000W | 4 hours |

These are theoretical estimates assuming a relatively constant load.

Actual performance depends on inverter consumption, battery temperature, load peaks and the appliances being powered.

### What could you power?

For example:

| Appliance | Typical consumption |
|---|---:|
| Refrigerator | 100–200W average |
| Freezer | 50–100W average |
| Internet router | 10–20W |
| LED lighting | 30–100W |
| Television | 80–150W |
| Laptop | 50–100W |
| Desktop computer | 150–400W |
| Circulation pump | 50–150W |

You should be able to run essential household equipment quite comfortably.

However, I would not initially include electric heating, electric boilers, induction cooking or EV charging in the backup circuit.

---

# 7. Estimated component costs

Here is an initial shopping budget based on currently available European retail listings.

These are indicative prices, not installation quotations.

| Component | Quantity | Estimated price |
|---|---:|---:|
| Victron MultiPlus-II 48/5000 | 1 | €700–1,050 |
| Pylontech US5000 | 2 | €1,740–1,900 |
| Cerbo GX MK2 | 1 | €240–320 |
| Battery rack | 1 | €200–400 |
| DC distribution and protection | 1 set | €250–450 |
| AC distribution and protection | 1 set | €200–350 |
| Cables and connectors | 1 set | €150–250 |
| Enclosure and mounting accessories | 1 set | €150–250 |

**Estimated hardware total: €3,630–4,970.**

This excludes installation, inspection and any necessary modifications to your existing electrical distribution board.

### Some actual products to investigate

**Inverter**

Victron MultiPlus-II 48/5000/70-50€708.58·uwsolarinstallatieshop + others4.8 (275)

**Battery**

Pylontech US5000 4.8kWh€869.99·reichelt.com + others5.0 (188)

**Monitoring**

Victron Cerbo GX€258.75·Bootschappen Watersport + others4.7 (119)

The Cerbo GX can also be replaced by an appropriate MultiPlus-II GX variant, which integrates GX functionality.

---

# 8. Preparing for solar panels later

Although we aren't installing solar panels initially, I would design the installation with solar expansion in mind.

The future architecture would look like this:

```text
                 SOLAR PANELS
                      |
                      |
                DC DISCONNECT
                      |
                      |
                 MPPT CONTROLLER
                      |
                      |
                      v
              +---------------+
              |               |
              |  48V BATTERY  |
              |               |
              +-------+-------+
                      |
                      |
                VICTRON INVERTER
                      |
                      |
                  230V OUTPUT
                      |
                      |
                HOUSEHOLD LOADS
```

This means you won't need to replace the original batteries or inverter.

You simply add the solar panels, MPPT controller and associated protection.

An alternative is to use a compatible AC-coupled solar inverter, depending on the final system design.

---

# 9. Belgian installation requirements

There is an important distinction between a completely isolated backup installation and an inverter/charger that remains connected to the public electricity network.

Even if you intend to use the batteries only during power failures, the installation topology matters.

For Belgium:

1. The installation must comply with AREI.
2. Grid-connected inverter equipment must meet applicable Synergrid requirements.
3. The appropriate installation and inspection requirements must be followed.
4. Registration with Fluvius may be required for the storage installation.

Check the official resources:

- [Synergrid C10/26 equipment list](https://www.synergrid.be/?utm_source=chatgpt.com)
- [Fluvius battery information](https://www.fluvius.be/?utm_source=chatgpt.com)
- [Victron MultiPlus-II technical documentation](https://www.victronenergy.com/inverters-chargers/multiplus-ii?utm_source=chatgpt.com)

I would specifically ask the installer to confirm that the proposed MultiPlus-II configuration is permitted in Belgium before ordering the equipment.

---

# 10. Three things I would like to establish before finalising the design

There are three technical details that could significantly change the design and cost.

### A. Single-phase or three-phase electricity?

This is particularly important in Belgium.

Do you have:

- 230V single-phase?
- 3 × 230V?
- 3 × 400V + N?

You can find this on your electricity meter or electricity installation documentation.

A single 48V MultiPlus-II provides single-phase 230V output. It does not independently provide a complete three-phase backup supply.

### B. What should happen during a power failure?

My preferred solution is to have a dedicated backup distribution board.

For example:

| Circuit | Backup |
|---|---|
| Refrigerator/freezer | Yes |
| Lighting | Yes |
| Internet/network | Yes |
| Television | Yes |
| Computer | Yes |
| General sockets | Selected |
| Washing machine | Optional |
| Dishwasher | No |
| Electric heating | No |
| EV charger | No |

Alternatively, we can investigate backing up the entire house, but this requires more careful power and phase calculations.

### C. Where will you install the batteries?

- Garage
- Basement
- Technical room
- Utility room

This affects the battery enclosure, temperature requirements, cable routing and installation costs.

---

## My preferred starting configuration

If the electrical supply allows it, I would start with:

| Component | Configuration |
|---|---|
| Battery | 2 × Pylontech US5000 |
| Storage | 9.6kWh nominal |
| Inverter | Victron MultiPlus-II 48/5000 |
| Monitoring | Cerbo GX |
| Battery rack | Six-battery capacity |
| Backup | Dedicated essential-circuits distribution board |
| Solar | Prepared for future installation |

**The goal:** Build a system once, start with approximately 10kWh, and expand to nearly 30kWh simply by adding batteries.

The most important question now is whether your home has single-phase or three-phase electricity. That determines which inverter architecture I would choose.

## Sources
- [Video Dead Simple Apocalypse Power: Build It in an Hour](https://www.youtube.com/watch?v=wqHAUnoCIQQ)
- [Details](https://willapproved.com/system-packages/handtruck-system-builder/)