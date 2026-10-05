# 2012 Kia Soul – Bosch Relay Unlock Booster (BCM Unlock Transistor Bypass)

**FULL DISCLOSURE: WHAT FOLLOWS IS ENTIRELY THEORY AND IS A WORK-IN-PROGRESS-- I HAVE NOT GOTTEN THIS WORKING YET, DESPITE NO LACK OF TRYING.** Further will note, nothing i did in 'testing' this live and searching for the correct terminal on the backside of their plugs caused any damage either. So no regressions, but no solution (yet) either.

This guide airms to restore full unlock functionality for the **passenger doors + hatch** using the **factory remote** and **interior lock/unlock switch**, bypassing the failed BCM unlock transistor by adding a Bosch-style relay.

---

# Overview

The BCM’s unlock transistor for the passenger doors commonly fails on 2010–2013 Kia Souls.  
Symptoms:

- Driver door unlocks normally
- Passenger doors + hatch **lock** normally
- Passenger doors + hatch **do NOT unlock**

This guide uses a Bosch relay to “boost” the BCM’s weak unlock pulse and send a full +12V unlock signal to the passenger unlock circuit.

---

# Parts & Tools

- Bosch-style SPDT relay (12V coil, 30/40A contacts)
- Inline fuse holder + 1–3A fuse
- 16–18 AWG wire (power/output)
- 18–20 AWG wire (relay coil)
- Posi-Tap connectors **or** solder + heat-shrink
- Trim tool, wire stripper, razor blade (if solder-tapping)
- Multimeter or test light
- Zip ties

---

# Wire Identification

### Unlock Wire (Passenger Doors + Hatch)

- **Color:** Yellow with Green stripe
- **Location:** Driver-side kick panel harness
- **Function:** Unlock signal for all passenger doors + hatch
- **Behavior:**
  - LOCK → +12V pulse
  - UNLOCK → weak/no pulse (BCM transistor failure)
- **Note:** Under bright light, the Green stripe often appears black. Verify with a meter.

### Constant +12V Source

Two options:

1. **Thick Red wire** in the same kick panel harness (recommended)
2. **Fuse tap** in interior fuse box (ROOM LP, DOOR LOCK 20a-- this is what i used with a fuse block tap wire, STOP LP)

---

## Bosch Relay Unlock Booster – Wiring Diagram (Revised)

+12V CONSTANT (FUSED 1–3A)
|
|
[30] ← Relay terminal 30 (blue EPAuto wire)
|
|
┌────┴────┐
| |
| |
[87] [87a]
(yellow) (red, unused)
|
|
(OUTPUT) →───┴───→ TAP INTO YELLOW/GREEN UNLOCK WIRE  
(Passenger doors + hatch unlock circuit)

Relay Coil (Trigger Side):

BCM Unlock Pulse → [86] (white EPAuto wire)
|
→ TAP INTO SAME YELLOW/GREEN WIRE  
(BCM’s weak unlock pulse triggers relay)

Ground → [85] (black EPAuto wire)

---

# Relay Terminal Mapping (Bosch + EPAuto Harness Colors)

| Relay Terminal | EPAuto Harness Color | Connect To               | Purpose                             |
| -------------- | -------------------- | ------------------------ | ----------------------------------- |
| **30**         | Blue                 | Constant +12V (fused)    | Power input for unlock pulse        |
| **85**         | Black                | Chassis ground           | Relay coil ground                   |
| **86**         | White                | Yellow/Green unlock wire | Relay trigger from BCM unlock pulse |
| **87**         | Yellow               | Yellow/Green unlock wire | Injects full +12V unlock pulse      |
| **87a**        | Red                  | _Not used_               | Leave empty                         |

## Important:

- **Terminal 86 (white)** and **terminal 87 (yellow)** both connect to the **same Yellow/Green wire**.
- They do _not_ connect to “in” and “out” sides of the fuse box.
- This relay augments the unlock pulse; it does not replace the internal relay.

---

# Best Relay Mounting Location

### Recommended Location:

**Behind the driver-side kick panel**, next to the harness containing the Yellow/Green unlock wire.

### Why this location is ideal:

- Unlock wire is right there
- Constant 12V wire is right there
- Multiple chassis grounds nearby
- Relay stays hidden and protected
- No long wire runs
- Easy to zip-tie to harness for OEM-style neatness

### Mounting Tips:

- Use a relay bracket **or** zip-tie the relay body to the harness
- Face terminals downward to reduce moisture exposure
- Keep wiring away from pedals and sharp edges

---

# Step-by-Step Installation Guide

## Step 1 — Access the Kick Panel Harness

1. Remove driver-side kick panel trim.
2. Locate the large harness running down from the interior fuse box.

## Step 2 — Identify the Yellow/Green Unlock Wire

1. Find the **Yellow with Green stripe** wire in the harness.
2. Verify with a meter:
   - LOCK → +12V pulse
   - UNLOCK → weak/no pulse
3. Note: The stripe may appear black under bright light.

## Step 3 — Identify a Constant +12V Source

Choose one:

**Option A — Thick Red Wire (recommended)**  
**Option B — Fuse Tap (ROOM LP, DOOR LOCK, STOP LP)**

Add a **1–3A inline fuse** before relay terminal 30 (blue).

## Step 4 — Mount the Relay

- Place relay behind kick panel.
- Secure with bracket or zip ties.
- Face terminals downward.

## Step 5 — Wire the Relay Coil (Trigger Side)

- **Terminal 86 (white)** → Tap into Yellow/Green unlock wire.
- **Terminal 85 (black)** → Chassis ground.

## Step 6 — Wire the Relay Contacts (Power Side)

- **Terminal 30 (blue)** → Fused +12V constant.
- **Terminal 87 (yellow)** → Tap into the **same Yellow/Green wire**.
- **Terminal 87a (red)** → Leave unused.

## Step 7 — Verify All Connections

- Yellow/Green wire tapped twice (86 + 87).
- Inline fuse installed.
- Ground secure.
- Wiring tidy.

## Step 8 — Test the System

1. Press **LOCK** → all doors lock normally.
2. Press **UNLOCK** →
   - Driver door unlocks
   - Relay clicks
   - Passenger doors + hatch unlock
3. Test interior lock/unlock switch.

## Step 9 — Reassemble Trim

- Tuck relay and wiring behind kick panel.
- Reinstall trim.
- Confirm nothing interferes with pedals.

---

# Result

You have successfully:

- Bypassed the failed BCM unlock transistor
- Restored full unlock functionality
- Preserved factory remote and switch behavior
- Avoided BCM replacement and programming
- Added a clean, OEM-like relay booster circuit
- Used correct EPAuto harness terminal mapping
- Ensured both relay coil trigger (86) and relay output (87) connect to the same Yellow/Green unlock wire.

This is the most reliable and elegant workaround for the 2012 Kia Soul unlock failure.

---

---

## Alternative Workaround: Manual Momentary Unlock Switch

As an alternative to the relay‑based unlock booster, you can restore passenger‑door + hatch unlocking by installing a **manual momentary pushbutton** that directly injects a +12V pulse into the passenger‑unlock circuit.

### How it works

The passenger unlock circuit on the 2012 Kia Soul is activated by a **+12V momentary pulse** on the **Yellow/Green wire** in the driver‑side kick‑panel harness. When the BCM’s unlock transistor fails, this pulse is no longer generated. A manual switch can provide this pulse on demand.

### Basic wiring layout

[Fused +12V] → [Momentary Pushbutton] → (Tap) → Yellow/Green Unlock Wire

### Behavior

- Pressing the button sends a +12V pulse to the unlock circuit.
- Passenger doors + hatch unlock immediately.
- Factory lock function remains unchanged.
- Driver door unlock continues to function normally via the BCM.

### Pros

- Extremely simple and inexpensive.
- No BCM programming or replacement required.
- No need to modify factory logic beyond a single wire tap.

### Cons

- Unlocking requires pressing the added button.
- Factory remote and interior switch will not unlock passenger doors.
- Less “OEM‑like” than the relay solution.

This option is ideal if you want the simplest possible fix and don’t mind a dedicated unlock button inside the cabin.
