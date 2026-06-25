# 56727D — VEX Robotics Competition Code

Competition robot software for **VEX V5**, 2025–2026 season. Team 56727D.

## What's Here

The codebase runs on a VEX V5 Brain and handles both autonomous and driver-control periods. Key systems:

- **Holonomic drivetrain** — four independently controlled motors (front-left, front-right, back-left, back-right) with a slow-mode toggle via controller button
- **Multi-stage intake** — five intake motors in sequence (bottom, two middle stages, high, and an unclog motor) for game element manipulation
- **Dual pneumatic pistons** — extend/retract toggle logic for each piston, independently bindable to controller buttons
- **Odometry** — full field position tracking using two tracking wheels (forward and sideways) plus an inertial sensor, updated continuously in a background thread with mutex locking to avoid race conditions

## Odometry

Position is tracked using the standard two-wheel odometry model:

- `forward_tracker` on PORT7 (reversed)
- `sideways_tracker` on PORT5
- `inertial_sensor` on PORT20 for heading

The odometry loop runs on a separate thread at a ~20ms cycle, computing `Δforward` and `Δsideways` deltas, then projecting them onto global `(x, y)` coordinates using the robot's current heading angle.

## Tech Stack

- **Language**: C++ (VEXcode V5)
- **Build system**: VEXcode makefile (`vex/mkenv.mk`)
- **Hardware**: VEX V5 Brain, motors, rotation sensors, inertial sensor, pneumatic actuators

## File Structure

```
src/
  main.cpp        # All robot logic — drivetrain, intake, pistons, odometry, competition tasks
include/
  vex.h           # VEX SDK header
vex/
  mkenv.mk        # VEXcode build toolchain
```

## Context

This is the full source for my team's competition robot. Writing competition robotics code pushed me to think seriously about real-time constraints — sensor polling rates, thread synchronization with mutexes, and the gap between a clean algorithm and one that actually holds up under match conditions.
