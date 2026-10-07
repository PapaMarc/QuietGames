# 2012 Kia Soul — Bosch Relay Unlock Workaround v2 (REVISED)

**Filename:** `2012KiaSoul_BoschRelayUnlockWorkaround_v2-REVISED.md`

## Purpose

This document describes a revised workaround for a 2012 Kia Soul in which:

- all doors lock normally;
- the driver's/front door still unlocks;
- the passenger/rear doors and hatch do not unlock;
- the suspected failure is the BCM output that operates the factory **Door Unlock Relay**;
- the goal is to restore the missing unlock function without replacing the BCM.

The important correction to the earlier [2012KiaSoul_BoschRelayUnlockWorkaround.md](2012KiaSoul_BoschRelayUnlockWorkaround.md) proposal is:

> **Do not attempt to supply +12 V directly to the passenger-door actuator circuit.**
>
> Instead, reproduce the missing **ground-side relay-control signal** that the BCM normally supplies to the factory Door Unlock Relay.

This leaves Kia's existing door-unlock relay and its factory motor-driving circuitry intact.

---

# 1. Factory circuit

The 2012 Soul's passenger-compartment fuse information identifies a 20-A **DR LOCK** circuit supplying:

- Door Lock Relay
- Door Unlock Relay
- 2 Turn Lock Relay

Source:

- [2012 Kia Soul Owner's Manual — DR LOCK fuse](https://cdn.dealereprocess.org/cdn/servicemanuals/kia/2012-soul.pdf)

The factory service-manual wiring section is:

- [2012 Kia Soul — Power Locks / SD813 wiring diagrams](https://charm.li/Kia/2012/Soul%20L4-1.6L/Repair%20and%20Diagnosis/Diagrams/Electrical%20Diagrams/Power%20Locks/)

The factory schematic is especially important because it shows that the BCM controls the lock/unlock relays rather than directly supplying the door-motor current.

The factory service-manual documentation also explains that connector diagrams are viewed from the terminal side unless otherwise specified:

- [Kia service-manual schematic/connector conventions](https://charm.li/Kia/2012/Soul%20L4-1.6L/Repair%20and%20Diagnosis/Restraints%20and%20Safety%20Systems/Diagrams/Diagram%20Information%20and%20Instructions/Schematic%20Diagrams/)

---

# 2. Relevant BCM outputs

The factory schematic identifies the BCM-side control circuits associated with the lock relays.

The working assumption for this workaround is:

| BCM circuit                           | Connector |    Pin | Wire                      | Function                                    |
| ------------------------------------- | --------: | -----: | ------------------------- | ------------------------------------------- |
| Door Lock Relay control               |     M04-A |      6 | 0.3 R/O                   | Lock                                        |
| Door Unlock Relay control             |     M04-A |  **7** | **0.3 L/O (Blue/Orange)** | **All/passenger-door unlock relay control** |
| 2-Turn Unlock / driver unlock control |     M04-A | **18** | **0.3 Br (Brown)**        | **Driver/front-door unlock relay control**  |

The critical signal for this workaround is therefore:

> **M04-A pin 7, Blue/Orange, 0.3 L/O**

which is associated with the factory Door Unlock Relay control.

The corresponding junction-block connection is:

> **I/P-MD pin 13**

The intended bypass is to momentarily pull this line to chassis ground, exactly as the failed BCM low-side output would have done.

---

# 3. Important wire-identification warning

There is conflicting secondary information on the internet concerning the Brown and Blue/Orange wires.

For example, an aftermarket-installation discussion for a 2012 Soul reports:

- Brown = all-door unlock
- Blue/Orange = driver unlock

See:

- [Fortin — 2012 Kia Soul unlock/lock installation discussion](https://www.fortin.ca/qa2/97092/2012-kia-soul-standard-key-rf641w-not-unlocking-and-locking-vehicle)

This conflicts with the interpretation of the factory SD813 schematic used in this document.

Therefore:

> **Do not permanently connect the circuit based on wire color alone.**

The actual connector pin number and electrical behavior on the vehicle being repaired must be verified.

In particular, before installing the bypass:

1. Identify **M04-A pin 7** from the connector/pin diagram.
2. Confirm that its wire is Blue/Orange.
3. Verify that the wire behaves as a relay-control line.
4. Confirm that momentarily grounding the line operates the factory Door Unlock Relay and unlocks the doors that currently fail to unlock.

If the actual vehicle's measured behavior contradicts the schematic interpretation, stop and resolve the discrepancy before installing the bypass.

---

# 4. Why the original Bosch-relay proposal was wrong

The earlier proposal connected:

```text
Relay 85 -> ground
Relay 86 -> Yellow/Green
Relay 30 -> +12 V
Relay 87 -> Yellow/Green
```

That arrangement attempts to use the same wire both as:

1. the relay trigger; and
2. the relay's switched output.

That does not replace a failed BCM output.

If the BCM output is the thing that has failed, there may be no usable trigger available on that wire.

More importantly, the factory circuit already contains the appropriate relay.

The BCM is supposed to control that relay; it is not supposed to directly provide the door-motor power.

Therefore the correct strategy is:

```text
        BCM
         |
         | failed low-side output
         |
   Blue/Orange
         |
   factory Door
   Unlock Relay
         |
       motors
```

Replace only the failed low-side control function:

```text
        BCM
         |
         | failed output
         X

   Blue/Orange
         |
   factory Door
   Unlock Relay
         |
       motors

        ^
        |
   added relay
        |
       GND
```

The added relay becomes the replacement ground switch.

---

# 5. Why use the working driver-unlock signal as the trigger?

The vehicle still successfully performs the driver/front-door unlock operation.

That provides a known-good unlock-related signal.

The intended architecture is therefore:

```text
Working driver-unlock signal
             |
             v
       low-current
       transistor
       interface
             |
             v
       added relay
             |
             v
      grounds the
      failed Door
      Unlock Relay
      control line
```

This avoids trying to obtain a positive unlock pulse from the failed circuit.

---

# 6. Why use a transistor interface instead of putting the Bosch relay coil directly on the BCM?

A typical Bosch-style 12-V automotive relay coil may draw approximately 120–160 mA depending on the particular relay and vehicle voltage.

That is an unnecessary additional load on the surviving Kia BCM driver.

Instead, the BCM output is used only to supply a few milliamps of **base current** to a PNP transistor.

The transistor then supplies the relay-coil current from the vehicle's +12-V supply.

Thus:

```text
BCM output
    |
    | ~3–4 mA
    v
PNP transistor
    |
    | ~100–160 mA
    v
added relay coil
```

The BCM sees only the small transistor-interface load.

---

# 7. Recommended transistor interface

## Components

Use:

| Designator | Component              | Value / part               |
| ---------- | ---------------------- | -------------------------- |
| Q1         | PNP transistor         | **BC327-40**               |
| R1         | Base pull-up           | **10 kΩ, 1/4 W**           |
| R2         | Base drive resistor    | **3.3 kΩ, 1/4 W**          |
| D1         | Relay flyback diode    | **1N4007**                 |
| K1         | Added automotive relay | 12-V coil, SPST-NO or SPDT |
| F1         | Added-circuit fuse     | **1 A recommended**        |

BC327-40 specifications include:

- PNP
- 45-V VCEO
- 800-mA collector-current rating
- industrial-grade TO-92 device

Reference:

- [BC327-40 technical data](https://diotec.com/en/product/BC327-40.html)
- [BC327 datasheet](https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/3986/BC327_Oct2014.pdf)

The 1N4007 is a 1-A general-purpose rectifier with a 1000-V reverse-voltage rating, more than adequate for suppressing a small automotive relay coil:

- [1N4007 specifications](https://www.voltaristech.com/product/1n4007)

---

# 8. Exact circuit

## Transistor/relay-driver side

```text
                         F1
                         1 A
                          |
                          |
                    switched +12 V
                          |
                          +----------------------+
                          |                      |
                          |                     E|
                          |                  Q1  |
                          |              BC327-40 |
                          |                     C|
                          |                      |
                          |                      +---------- K1 pin 86
                          |                                   |
                          |                              [ RELAY COIL ]
                          |                                   |
                          |                                   |
                          +---- R1 10 kΩ ----+                |
                                            |                |
                                            +---- B           |
                                            |                |
                                       Q1 base                |
                                            |                |
                                            R2 3.3 kΩ         |
                                            |                |
                                            +----------------+
                                            |
                              working driver-unlock
                              low-side control signal
                                            |
                                    BCM M04-A pin 18
                                    Brown*
```

Where:

- Q1 emitter → fused/switched +12 V
- Q1 collector → K1 relay-coil positive
- R1 = 10 kΩ from Q1 base to Q1 emitter
- R2 = 3.3 kΩ from Q1 base to the working unlock-control signal
- K1 relay-coil negative → chassis ground

- **The Brown/pin-18 assignment must be verified against the actual vehicle before connection because secondary sources conflict with the schematic interpretation.**

---

# 9. Relay flyback diode

Place D1 directly across K1's coil:

```text
                    K1 pin 86
                         |
                         +---------+
                         |         |
                       [ COIL ]   |<|
                         |        D1
                         |     1N4007
                         |         |
                         +---------+
                                   |
                              K1 pin 85
                                   |
                                  GND
```

**D1 orientation is critical:**

```text
D1 cathode / banded end -> K1 pin 86 / +12 V
D1 anode               -> K1 pin 85 / ground
```

The diode normally does nothing.

When Q1 turns K1 off, the collapsing magnetic field in the relay coil creates a reverse-voltage spike. D1 provides a safe current path and prevents that spike from appearing across Q1.

---

# 10. K1 contact wiring

The added relay does **not** power the door motors.

Its contacts only reproduce the missing ground signal.

```text
                   CHASSIS GROUND
                         |
                         |
                       K1-30
                         |
                    [ ADDED RELAY ]
                         |
                       K1-87
                         |
                         |
              Blue/Orange 0.3 L/O
                         |
                    M04-A pin 7
                         |
                    I/P-MD pin 13
                         |
                FACTORY DOOR UNLOCK
                     RELAY CONTROL
```

K1-87a is unused if using a SPDT relay.

The critical connection is therefore:

> **K1-87 → M04-A pin 7 / Blue-Orange / I/P-MD pin 13**

and:

> **K1-30 → chassis ground**

The added relay is therefore electrically equivalent to adding a replacement low-side transistor for the failed BCM output.

---

# 11. Why 3.3 kΩ?

The selected R2 value is a compromise between:

- minimizing current drawn from the surviving BCM driver; and
- providing enough base current to switch K1 reliably.

Assume a nominal 12-V supply and approximately 0.7 V base-emitter voltage:

```text
I_B = (12.0 V - 0.7 V) / 3300 Ω

I_B ≈ 3.42 mA
```

At 14.4 V:

```text
I_B = (14.4 V - 0.7 V) / 3300 Ω

I_B ≈ 4.15 mA
```

Thus the BCM output supplies only approximately:

> **3.4–4.2 mA**

to the transistor base.

That is dramatically less than the ~120–160 mA that a conventional Bosch relay coil may otherwise impose directly on the BCM.

---

# 12. Relay-current calculation

For illustration, consider a 90-Ω relay coil.

At 12.0 V:

```text
I_coil = 12.0 V / 90 Ω
       ≈ 133 mA
```

At 14.4 V:

```text
I_coil = 14.4 V / 90 Ω
       = 160 mA
```

With approximately 3.4–4.2 mA of base current, the corresponding forced-current ratios are approximately:

```text
12 V:
133 mA / 3.42 mA ≈ 39

14.4 V:
160 mA / 4.15 mA ≈ 39
```

So the circuit is deliberately designed around approximately a **forced beta of 40**.

The BC327-40 has substantially higher specified DC gain than this under its normal test conditions, although the transistor should not be treated as a substitute for an automotive-qualified relay driver merely because its headline gain is high.

Reference:

- [BC327 datasheet](https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/3986/BC327_Oct2014.pdf)

If a particular relay has a substantially lower coil resistance than approximately 90 Ω, its actual coil current must be checked before using the 3.3-kΩ value.

---

# 13. Why not simply use 2.2 kΩ?

A 2.2-kΩ resistor would provide more base current:

```text
(12.0 - 0.7) / 2200 ≈ 5.1 mA
```

but that additional current is unnecessary if K1 operates reliably with the 3.3-kΩ resistor.

The 3.3-kΩ value therefore intentionally minimizes the load placed on the working BCM output.

If a particular relay fails to pull in reliably at the lowest vehicle voltage, R2 can be reduced to 2.2 kΩ.

Do not go substantially lower without reconsidering the current being demanded from the BCM output.

---

# 14. Why the added relay is preferable to modifying the good Kia relay

A tempting alternative is to interrupt or replace the factory relay that currently performs the working driver/front-door unlock.

That is undesirable.

The factory system already has a functioning relay and driver-door path.

The objective is to leave that path completely intact:

```text
             WORKING FACTORY PATH

BCM
 |
 +----> 2-Turn / driver-unlock relay
              |
              +----> driver/front door
```

and add a parallel replacement only for the failed path:

```text
             FAILED PATH

BCM
 |
 X  failed low-side output
 |
 +----> Door Unlock Relay
              |
              +----> passenger/rear doors


             REPAIR

working driver-unlock signal
             |
             v
         Q1 interface
             |
             v
           K1 relay
             |
             v
         ground Blue/Orange
             |
             v
       Door Unlock Relay
             |
             +----> passenger/rear doors
```

This is the central reason for using the BC327 + small-resistor interface rather than rewiring the factory relay arrangement.

It preserves the good Kia hardware and changes only the failed control function.

---

# 15. Diagnostic test before installing the circuit

Before building the bypass, perform the following test.

**This is a diagnostic test, not a permanent connection.**

After positively identifying the suspected factory Door Unlock Relay control wire:

> **M04-A pin 7 / Blue-Orange / I/P-MD pin 13**

momentarily connect that line to chassis ground.

Use a small fused jumper; approximately **1 A** is sufficient for this diagnostic connection because the line is expected to be controlling a relay coil rather than the door motors.

Expected result:

```text
Blue/Orange grounded
       |
       v
factory Door Unlock Relay operates
       |
       v
passenger/rear doors unlock
```

If this happens, it strongly confirms that:

1. the factory Door Unlock Relay works;
2. the downstream door-lock wiring works;
3. the passenger/rear actuator circuit works; and
4. the missing function is the BCM-side low-side control.

If grounding the line does **not** unlock the affected doors, do not install the bypass yet. The fault may be in the relay, junction box, wiring, actuator circuit, or identification of the control wire.

---

# 16. Important safety/verification requirements

Before permanent installation:

- Disconnect the negative battery terminal while making permanent harness connections.
- Confirm the connector/pin identification rather than relying on wire color.
- Confirm that the intended BCM control line is a low-side relay-control circuit.
- Never inject +12 V into the BCM control line.
- Never connect the Brown and Blue/Orange wires directly together.
- Use an appropriately fused +12-V supply for the added electronics.
- Insulate all exposed connections.
- Secure the added module so it cannot contact metal or pedals.
- Keep the repair away from SRS/airbag wiring and connectors.
- Verify lock and unlock operation several times before returning the vehicle to service.

---

# 17. Expected operation of the simple version

If the working driver-unlock control is used as the trigger, the resulting behavior is expected to be:

```text
Driver-unlock signal
        |
        v
      Q1 ON
        |
        v
      K1 ON
        |
        v
Blue/Orange grounded
        |
        v
Factory Door Unlock Relay ON
        |
        v
Passenger/rear doors unlock
```

This version intentionally prioritizes a simple, robust hardware repair over reproducing every nuance of the original two-stage unlock logic.

Depending on the exact BCM programming and which signal is used as the trigger, the repair may cause a single remote-unlock command to unlock all doors rather than retaining the original first-press/second-press behavior.

---

# 18. Original GitHub proposal vs. revised design

## Original proposal

```text
BCM/vehicle wire
      |
      +---- relay coil
      |
      +---- relay output

relay also supplies +12 V
to the same wire
```

Problems:

- same wire used as trigger and output;
- assumes the failed signal can still drive the added relay;
- attempts to manipulate the downstream voltage rather than replacing the failed relay-control function;
- risks backfeeding the BCM;
- bypasses rather than preserves the factory relay architecture.

Reference:

- [Original `2012KiaSoul_BoschRelayUnlockWorkaround.md`](https://github.com/PapaMarc/QuietGames/blob/main/vehicles/2012KiaSoul_BoschRelayUnlockWorkaround.md)

## Revised proposal

```text
WORKING BCM UNLOCK SIGNAL
          |
          | only ~3–4 mA
          v
      BC327-40
          |
          | relay coil current
          v
       added K1
          |
          | ground
          v
  Blue/Orange Door-Unlock
     Relay-Control line
          |
          v
  FACTORY Kia Door Unlock
         Relay
          |
          v
 passenger/rear actuators
```

This is fundamentally different:

> **The added relay is not replacing Kia's Door Unlock Relay. It is replacing the failed BCM transistor that was supposed to operate that relay.**

---

# 19. Parts list

### Required

- 1 × **BC327-40 PNP transistor**
- 1 × **3.3 kΩ, 1/4-W resistor**
- 1 × **10 kΩ, 1/4-W resistor**
- 1 × **1N4007 diode**
- 1 × **12-V automotive SPST-NO or SPDT relay**
- 1 × **1-A fuse and holder**
- suitable automotive wire/connectors
- heat-shrink insulation

### Relay requirements

The added relay does not carry door-motor current.

A normal automotive relay is more than adequate, provided its coil operates from the vehicle's 12-V system and its coil current is compatible with the BC327 driver.

The important specification is therefore the **coil resistance/current**, not the headline 30-A/40-A contact rating.

---

# 20. Bottom line

The corrected concept is:

```text
                  WORKING BCM OUTPUT
                         |
                    M04-A pin 18*
                         |
                       3.3k
                         |
                    BC327-40
                         |
                     added K1
                         |
                  30 -> GND
                  87 -> Blue/Orange
                         |
                    M04-A pin 7
                    I/P-MD pin 13
                         |
                 factory Door Unlock
                       Relay
                         |
                  passenger/rear
                     actuators
```

The BC327-40 + 3.3-kΩ + 10-kΩ + 1N4007 arrangement is chosen specifically so that the surviving BCM output supplies only approximately **3.4–4.2 mA** of base-drive current while the transistor supplies the much larger relay-coil current.

That preserves the working Kia relay/driver-door circuit rather than interrupting or replacing it.

**The one unresolved item that must be verified on the actual vehicle is the Brown-versus-Blue/Orange functional assignment.** The factory service-manual schematic and at least one aftermarket 2012 Soul installation source disagree on that wire-color interpretation. The connector pin number and measured low-going behavior should therefore take precedence over color alone.

---

## References

1. [Kia 2012 Soul Owner's Manual — fuse information](https://cdn.dealereprocess.org/cdn/servicemanuals/kia/2012-soul.pdf)
2. [Kia 2012 Soul — Power Locks / SD813-1, SD813-2 and connector pinouts](https://charm.li/Kia/2012/Soul%20L4-1.6L/Repair%20and%20Diagnosis/Diagrams/Electrical%20Diagrams/Power%20Locks/)
3. [Kia 2012 Soul — schematic and connector-view conventions](https://charm.li/Kia/2012/Soul%20L4-1.6L/Repair%20and%20Diagnosis/Restraints%20and%20Safety%20Systems/Diagrams/Diagram%20Information%20and%20Instructions/Schematic%20Diagrams/)
4. [Fortin — 2012 Kia Soul lock/unlock wire discussion](https://www.fortin.ca/qa2/97092/2012-kia-soul-standard-key-rf641w-not-unlocking-and-locking-vehicle)
5. [BC327-40 technical specifications](https://diotec.com/en/product/BC327-40.html)
6. [BC327 datasheet](https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/3986/BC327_Oct2014.pdf)
7. [1N4007 specifications](https://www.voltaristech.com/product/1n4007)
8. [Original GitHub workaround](https://github.com/PapaMarc/QuietGames/blob/main/vehicles/2012KiaSoul_BoschRelayUnlockWorkaround.md)
