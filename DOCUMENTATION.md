# MovingPlatform — Full Documentation

This document explains all configuration options, platform modes, signal behavior, and the activator/interactable system.

---

## Table of Contents
- [Setup Instructions](#setup-instructions)
- [Configuration](#configuration)
- [Platform Types](#platform-types)
- [Signals](#signals)
- [Methods](#methods)
- [Activator System](#activator-system)

---

## Setup Instructions

### Basic Setup
1. Attach the `MovingPlatform` script to an `AnimatableBody2D`.
2. Add a `Marker2D` child to define the destination point.
3. Assign the following:
   - `final_pos_marker`
   - `platform`
4. (Optional) Add a `Line2D` child and assign it to `platform_line` for path visualization.

### Configuration Resources
1. Create a **MovingPlatformConfig** resource and assign it to `config`.
2. Create a **TweenResource** and assign it to `tween_resource`.
3. Configure movement mode, delay, and stop-frame in the config resource.
4. Configure duration, easing, and process mode in the tween resource.

### Activator Setup (Triggered Types)
1. Add an `Activator` node or reference an existing one.
2. The platform automatically connects to its activation signals.
TRIGGERED, TOGGLE, or ONE_WAY platforms require an Activator

---

## Configuration

### MovingPlatformConfig Properties
- **type** — Platform movement type (LOOP, TRIGGERED, TOGGLE, ONE_WAY)
- **delay** — Delay before initial movement
- **stopframe** — Pause duration at each end for LOOP
- **subsequent_delay** — Delay before triggering child/linked platforms
- **backwards_scale** — Speed multiplier for reverse movement
- **move_on_ready** — LOOP: automatically start on `_ready()`
- **move_on_screen_entered** — Start when entering the viewport

### TweenResource Properties
- **base_duration** — Base time for movement
- **trans** — Tween transition type
- **ease** — Tween easing function
- **process_mode** — Idle or physics tweening

---

## Platform Types

### **LOOP**
- Moves continuously between start/end points
- Can auto-start on ready or screen enter
- Supports endpoint stopframe delay

### **TRIGGERED**
- Moves forward when activated
- Holds position when deactivated
- Requires an Activator

### **TOGGLE**
- Moves forward when activated, backward when deactivated
- Reverse speed affected by `backwards_scale`
- Requires an Activator

### **ONE_WAY**
- Moves to final position and remains there
- No backward movement
- Requires an Activator

---

## Signals

- **stopped** — Emitted when the platform fully stops
- **moving_forward** — Emitted when movement begins toward the final position
- **moving_backward** — Emitted when moving back toward the start

---

## Methods

### `move()`
Starts movement depending on platform type. LOOP begins continuous movement; triggered types move toward the target.

### `play_backwards()`
Moves the platform back toward its starting position. Uses `backwards_scale`.

### `_update_path_visualization()`
Refreshes the Line2D path between the platform and target point.

### `_setup_connections()`
Automatically connects to activator signals based on platform type.

---

## Activator System

### Interactable Base Class
The platform extends an `Interactable` class that provides:
- An exported `Activator` reference
- Automatic signal connection to `activated`/`deactivated`
- `is_active` boolean state
- Overridable `_activated()` and `_deactivated()` functions

### Included Button Scene
A prebuilt button activator is included:

- **Path**: `res://addons/your_plugin_name/Scenes/InteractableButton.tscn`
- Includes Area2D, CollisionShape2D, and visuals
- Automatically activates when bodies enter/exit the area

### Custom Activators
To create a custom activator:
1. Inherit from `Activator`
2. Emit the proper activation signals
3. Assign your activator to the platform in the Inspector

The platform will handle the connection automatically.

---
