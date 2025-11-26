# MovingPlatform

A lightweight configurable moving platform system for Godot 4 featuring resource-driven movement, multiple platform modes, and an extensible activator/trigger system.

Inspired by [LukeCGG Moving Platforms Plugin](https://github.com/LukeCGG/MovingPlatformsAnimatedEASY)

This repository contains:
- A demo scene showing common use cases  
- Full documentation explaining configuration, platform types, and the activator system

**To get started quickly, open the demo project.**  
**For detailed usage, see [DOCUMENTATION.md](DOCUMENTATION.md).**

## Features
- **Resource-Driven Configuration**  
  Movement logic and tweening behavior live in separate reusable resource files.

- **Multiple Platform Behaviors**  
  LOOP, TRIGGERED, TOGGLE, and ONE_WAY modes.

- **Path Visualization**  
  Optional Line2D preview showing the platform’s travel path.

- **Signal-Based Communication**  
  Emits movement state signals for easy integration.

- **Activator / Interactable System**  
  Use built-in buttons or create custom activators.

## Quick Setup
1. Add a `MovingPlatform` script to an `AnimatableBody2D`.
2. Add a child `Marker2D` → assign as the destination.
3. Add a `Line2D` (optional) → assign as path visualizer.
4. Create:
   - `MovingPlatformConfig` resource
   - `TweenResource` resource  
   Assign both via the Inspector.

See full instructions in **DOCUMENTATION.md**.

## Requirements
- **Godot 4.x**
- This plugin/scene pack is fully self-contained.

## License
MIT

