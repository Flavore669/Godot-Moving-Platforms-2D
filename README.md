# MovingPlatform

A configurable moving platform system for Godot 4 that supports multiple movement behaviors and timing controls through external resources. Built on an interactable activator system for flexible triggering mechanisms.

## Key Traits

- **Resource-Driven Configuration**: Movement behavior and timing/easing are separated into dedicated resource classes
- **Multiple Platform Types**: Supports looping, triggered, toggle, and one-way movement patterns
- **Visual Path Indicators**: Built-in line visualization for platform movement paths
- **Signal-Based Communication**: Emits signals for movement state changes
- **Coordinated Platform Systems**: Supports delayed sequential activation of other platforms
- **Screen-Based Activation**: Optional movement triggered by screen entry
- **Interactable Foundation**: Built on extensible activator/interactable system for various trigger types

## Table of Contents

- [Setup Instructions](#setup-instructions)
- [Configuration](#configuration)
- [Platform Types](#platform-types)
- [Signals](#signals)
- [Methods](#methods)
- [Dependencies](#dependencies)
- [Activator System](#activator-system)

## Setup Instructions

### Basic Setup
1. Attach the `MovingPlatform` script to an `AnimatableBody2D` node
2. Create a `Marker2D` as a child node to define the platform's destination
3. Assign the `final_pos_marker` and `platform` references in the inspector
4. Add a `Line2D` node as a child for path visualization and assign to `platform_line`

### Configuration Resources
1. Create a `MovingPlatformConfig` resource and assign to the `config` property
2. Create a `TweenResource` resource and assign to the `tween_resource` property
3. Configure movement behavior in the config resource and timing in the tween resource

### Activator Setup (for triggered platforms)
1. Add an `Activator` node as a child or reference an existing one
2. The platform will automatically connect to the activator's signals for TRIGGERED and TOGGLE types

## Configuration

### MovingPlatformConfig Properties
- **type**: Defines movement behavior (LOOP, TRIGGERED, TOGGLE, ONE_WAY)
- **delay**: Initial delay before movement starts
- **stopframe**: Pause duration at each end point for LOOP types
- **subsequent_delay**: Delay before activating other platforms in the chain
- **backwards_scale**: Speed multiplier for reverse movement in TOGGLE types
- **move_on_ready**: Whether LOOP platforms start automatically
- **move_on_screen_entered**: Whether to move when entering the screen

### TweenResource Properties
- **base_duration**: Base time for movement calculations
- **trans**: Tween transition type
- **ease**: Tween easing function
- **process_mode**: Physics or idle processing

## Platform Types

### LOOP
- Continuously moves between start and end positions
- Can start automatically on ready or screen entry
- Uses stopframe for pauses at each endpoint

### TRIGGERED
- Moves forward when activated, pauses when deactivated
- Stays at current position when deactivated
- Requires an Activator node for functionality

### TOGGLE
- Moves forward when activated, backwards when deactivated
- Backwards movement speed controlled by backwards_scale
- Requires an Activator node for functionality

### ONE_WAY
- Moves to end position when activated and stays there
- No reverse movement capability
- Requires an Activator node for functionality

## Signals

- `stopped`: Emitted when platform comes to complete stop
- `moving_forward`: Emitted when platform starts moving toward final position
- `moving_backward`: Emitted when platform starts moving toward original position

## Methods

### `move()`
Initiates platform movement based on configured type. For LOOP platforms, starts continuous movement. For triggered types, begins movement toward destination.

### `play_backwards()`
Moves platform back to original position. Used by TOGGLE platforms when deactivated. Respects backwards_scale for movement speed.

### `_update_path_visualization()`
Updates the Line2D visualization to show movement path between current position and destination marker.

### `_setup_connections()`
Automatically connects to activator signals for TRIGGERED and TOGGLE platform types. Called during initialization and when activator is assigned.

## Activator System

### Interactable Base Class
The platform extends the `Interactable` base class which provides:

- **Activator Reference**: `@export var activator : Activator` for connecting trigger sources
- **Automatic Signal Connection**: Automatically connects to activator's activated/deactivated signals
- **State Management**: Maintains `is_active` boolean state
- **Override Methods**: `_activated()` and `_deactivated()` for custom behavior

### Included Button Scene
The plugin includes a pre-configured button scene that can be used as an activator:

- **Path**: `res://addons/your_plugin_name/Scenes/InteractableButton.tscn`
- **Components**: Comes with Area2D detection, collision shape, and basic visuals
- **Usage**: Simply instantiate the scene and connect it to your moving platform's activator property

The button scene uses the `InteractableButton` script which automatically handles activation when bodies enter/exit its detection area, providing immediate functionality without additional setup.

### Custom Activators
Create custom activators by extending the `Activator` base class and implementing your own trigger conditions. The platform will automatically connect to any assigned activator.
