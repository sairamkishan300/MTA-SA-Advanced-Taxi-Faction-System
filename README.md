# MTA:SA Advanced Taxi Faction System

A fully-featured taxi job / faction system for Multi Theft Auto: San Andreas (MTA:SA).

This resource provides an immersive passenger–driver flow with an in-game tablet UI, booking list for taxi drivers, automatic fare calculation, checkpoints, and notifications. It is designed to be plug-and-play and easy to customize for roleplay servers.

---

## Features

- **Taxi booking command**
  - Passengers use the `/taxi` command to open a modern tablet-style UI.
  - Shows all online taxi drivers (based on ACL group).

- **Passenger-side tablet UI**
  - Clean iPad-style interface using custom images.
  - Driver list panel.
  - Location selection screen with map integration.
  - Book / cancel ride buttons with hover animations.

- **Driver-side taxi faction panel**
  - Opened via the `F4` key for players in the taxi ACL group.
  - Lists all active bookings with:
    - Player ID
    - Account name
    - Distance (meters)
    - Fare (calculated automatically)
  - One-click booking acceptance.

- **Automatic fare system**
  - Fare is calculated based on distance between passenger position and chosen drop location.
  - Configurable `taxiFeePerMeter` in `config.lua`.

- **Teleport to closest checkpoint (optional helper)**
  - When a driver accepts a booking, the script finds the closest configured checkpoint to the passenger.
  - If the driver is far enough, their vehicle is teleported there (anti-abuse distance configurable).

- **Pickup and drop checkpoints**
  - Custom markers and blips only visible to the relevant driver.
  - Separate pickup and drop markers.
  - Configurable marker size, hit radius, and blip appearance.

- **Notification integration**
  - Uses `add:notification` client events for consistent UI notifications.
  - Informative messages for passengers and drivers at each step.

- **Config-driven**
  - All important values (ACL group name, fee per meter, checkpoints, etc.) are centralized in `config.lua`.

---

## Files Overview

Inside the `taxi` resource folder:

- **`meta.xml`** – MTA resource meta definition (scripts & assets).
- **`config.lua`** – Configuration file for ACL group, fare, checkpoints, marker/blip settings, etc.
- **`taxi_s.lua`** – Server-side logic:
  - Handles `/taxi` command.
  - Manages bookings list.
  - Taxi acceptance and teleport logic.
  - Creation of pickup/drop checkpoints and blips.
- **`taxi_c.lua`** – Client-side logic & UI:
  - Tablet interface (passenger and taxi faction panels).
  - Map integration for drop location selection.
  - Input handling (F4 key, mouse, map clicks).
- **`[images]/`** – UI assets (tablet body, background, buttons, icons).
- **`[font]/font.ttf`** – Custom font for the UI.

---

## Requirements

- **MTA:SA Server** 1.5+ (tested on current stable builds).
- **ACL access** to create / edit groups.
- A **login system** that sets an element data or account for players:
  - The script uses `getPlayerAccount`, `getAccountName`, and `getElementData(player, "id")` (or similar) for player identification.
- A **notification system** that listens to `add:notification` event (or replace those calls with your own messages).

---

## Installation

1. **Copy the resource**
   - Place the `taxi` folder into your server resources directory, e.g.:
   - `mods/deathmatch/resources/[factions]/[taxi]/taxi`

2. **ACL setup**
   - In your ACL config, create a taxi group (or reuse an existing one), for example:
     - Group name: `Taxi`
   - Add the appropriate accounts to this group.
   - Make sure the group name matches `taxiACLGroup` in `config.lua`.

3. **Resource dependencies (if any)**
   - Ensure your login / notification systems are running before this taxi resource.
   - If you use custom exports for notifications, adapt the event calls in the scripts.

4. **Add to `mtaserver.conf`**
   - Register the resource so it auto-starts:
   
   ```xml
   <resource src="taxi" startup="1" protected="0" />
   ```

5. **Start the resource**
   - From the server console or admin panel:
   
   ```
   start taxi
   ```

---

## Configuration

All main configuration is in `config.lua`.

Key options:

- **`taxiACLGroup`**
  - Name of the ACL group whose members are considered taxi drivers.

- **`taxiFeePerMeter`**
  - Fare rate per meter.
  - The final price is `distance * taxiFeePerMeter`.

- **`checkpoints`**
  - A list of `{x, y, z}` positions used as teleport helper locations near passengers.
  - The script picks the closest checkpoint to the passenger.

- **`PhoneNumberElementData`**
  - Name of the element data key where player phone number / ID is stored.
  - Shown in the booking details and messages.

- **`minDistanceToTeleportDriver`**
  - If the distance between driver and passenger is less than this value, the script will **not** teleport the driver (to prevent abuse).

- **Marker & Blip**
  - `taxiMarkerSize` – Size of checkpoint markers.
  - `taxiMarkerHitRadius` – Radius used to detect when driver reached the marker.
  - `taxiBlipSize` – Map / radar blip size.
  - `taxiBlipAlpha` – Transparency of marker & blip (0–255).

Adjust these values to fit your server’s balance and map layout.

---

## Usage

### For Passengers

- **Command**: `/taxi`
  - Opens the passenger tablet UI.
- **Steps**:
  - Open `/taxi`.
  - Select drop location using the map screen.
  - Confirm booking.
  - Wait for a taxi driver to accept your booking.
  - Follow on-screen notifications until you are picked up and dropped.

### For Taxi Drivers

- **Requirement**: Must be in the `Taxi` ACL group (or the name you configured).
- **Key**: `F4`
  - Opens the taxi faction panel.
- **Steps**:
  - Press `F4` to view active bookings.
  - Click a booking to accept it.
  - Follow the created checkpoint to the passenger, then to the drop location.
  - Coordinate payment with the passenger (the script displays fare information; actual money transfer can be integrated with your economy).

---

## Customization & Integration

- **Economy integration**
  - Currently, the script calculates fare and shows it to driver & passenger.
  - You can hook into your money system (e.g., `takePlayerMoney` / `givePlayerMoney` or custom exports) inside server events where the drop is completed.

- **Notification system**
  - If you don’t use `add:notification`, replace these events with your own notification or chat system.

- **UI / Branding**
  - Replace images in the `[images]` folder with your own (keep the same filenames or update paths in `taxi_c.lua`).
  - Replace the font in `[font]/font.ttf` if you want a different style.

---

## Credits

- **Script author**: SAI RAM KISHAN K J  
  Discord: `sairamkishan#9290`  
  Instagram: `@sairamkishan30`

If you resell or redistribute this resource, please keep the credit line to the original author, unless you have explicit permission to remove or modify it.

---

## License

Choose and add your own license terms here depending on how you want to sell or distribute this script (e.g. "All rights reserved", custom commercial license, etc.).

For example:

> All rights reserved. You are not allowed to resell or redistribute this resource without explicit written permission from the author.
