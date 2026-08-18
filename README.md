# Pickup And Drop System (Unreal Engine)

A Blueprint-only Unreal Engine 5 project demonstrating a hand-socket based **pickup, carry, drop, and place** interaction system on top of the Third Person template — the kind of core mechanic used in adventure, immersive-sim, and puzzle games.

Project name: `testingPickupSystem` · Engine: **Unreal Engine 5.8**

## Features

- **Pickup / Drop via component** — a reusable `BP_Pickup_Component` added to the character handles picking up and dropping world items, and tracks whether a hand currently has an item held.
- **Dual hand-socket support** — items attach to the mannequin's `HandGrip_L` or `HandGrip_R` skeletal socket, selectable through a custom `E_Hand` enum (Left / Right).
- **Interactable items** — `BP_PickupItem` is the base actor for anything the player can pick up, built around an overlap-based trigger.
- **Place / Snap system** — `BP_PlaceActor` lets a carried item be placed back down and snapped to a target location (`SnapToTarget`), rather than just dropped anywhere — useful for "return the item to its spot" style objectives.
- **Shared interaction interface** — `BPI_Intract`, a Blueprint Interface implemented by interactable actors so the character can call a generic interact function without caring about the concrete class.
- **HUD interface** — `BPI_HUDIntarface` plus a `WBP_Main` UMG widget for on-screen prompts/feedback.
- **Enhanced Input** — Move, Look, Mouse Look, Jump, and a dedicated **Drop** input action, alongside the standard third-person control set.
- **Level prototyping kit** — grid-textured prototyping meshes/materials plus example interactables (a door, a jump pad, a wobble target) for blocking out and testing interaction ranges before final art passes.
- **Demo pickup asset** — a broom (`BP_Broom`), imported via Fab, used as the example item you pick up, carry, and place in the sample level.

## Project Structure

```
Content/
├── Pickup/
│   ├── BP_PickupItem.uasset        # Base pickup-able item actor
│   ├── BP_Pickup_Component.uasset  # Attach to character: handles pickup/drop, hand sockets
│   └── E_Hand.uasset                # Left / Right hand enum
├── Place/
│   └── BP_PlaceActor.uasset         # Snap-to-target placement logic
├── Asset_BP/
│   └── BP_Broom.uasset              # Example pickup item (broom)
├── BPI_Intract.uasset               # Shared "interact" Blueprint Interface
├── BPI_HUDIntarface.uasset          # HUD callback interface
├── WBP_Main.uasset                  # Main HUD widget
├── ThirdPerson/
│   ├── Blueprints/                  # Character, GameMode, PlayerController
│   └── Lvl_ThirdPerson.umap         # Default/startup level
├── Input/
│   ├── Actions/                     # IA_Move, IA_Look, IA_Jump, IA_Drop, ...
│   └── IMC_Default / IMC_MouseLook  # Input Mapping Contexts
├── Characters/Mannequins/           # Standard UE5 mannequin skeleton, meshes, anims
├── LevelPrototyping/                # Grid materials + prototyping meshes and interactables
│   └── Interactable/                # Door, JumpPad, WobbleTarget examples
└── Fab/                             # Marketplace assets (broom mesh, Megascans surfaces)
```

## Getting Started

### Prerequisites

- **Unreal Engine 5.8** (or a compatible 5.x version — update `EngineAssociation` in `testingPickupSystem.uproject` if you're on a different install)
- No Visual Studio / C++ toolchain required — this is a **Blueprint-only** project (no `Source/` folder)

### Running the Project

1. Clone the repo.
2. Double-click `testingPickupSystem.uproject`, or open it from the Epic Games Launcher / Unreal Editor.
3. The project opens directly into `Lvl_ThirdPerson`, the default and startup map.
4. Press **Play** — walk up to the broom, pick it up, and use the **Drop** input to let it go, or walk it to its placement spot to snap it into place.

### Controls

| Action | Input |
|---|---|
| Move | WASD |
| Look | Mouse |
| Jump | Spacebar |
| Pick up / Interact | Overlap with a pickup-enabled item |
| Drop | Bound to `IA_Drop` |

## How It Works

- `BP_Pickup_Component` lives on the character and is the single source of truth for what's currently held: it stores which hand is occupied (via `E_Hand`), attaches the picked-up mesh to the corresponding `HandGrip_L`/`HandGrip_R` socket, and exposes `DropItem` to release it.
- `BP_PickupItem` is the base class for any world object that can be picked up; interactable actors implement the shared `BPI_Intract` interface so the character doesn't need per-item logic.
- `BP_PlaceActor` extends the drop flow with placement: instead of items falling wherever they're released, they can be aligned and snapped to a designated target transform, enabling "put it back" style objectives.
- The `LevelPrototyping` content (grid materials, chamfer cubes, a door, a jump pad, a wobble target) is greybox/prototyping kit used to block out and test interaction spaces alongside the pickup system.

<img width="1411" height="478" alt="gameplay" src="https://github.com/user-attachments/assets/8b0c42cf-3e4c-43e5-b41d-b375e37cd816" />





