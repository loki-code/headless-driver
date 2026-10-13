# Headless Driver (SA-MP / open.mp) — binary release

AI cars with no NPC driver, for SA-MP and open.mp servers. The plugin handles route following,
steering, speed control, braking and avoidance, and drives ordinary vehicles with it. It does two
jobs:

1. **Ambient traffic** — cars spawn on road nodes around players, wander the road network forever,
   and are removed again when nobody is near.
2. **Scripted drivers** — give a vehicle you own a fixed route to drive.

This is a **binary-only release**: no source code is included or available. See `LICENSE.txt`
before using this in your server.

## What's in this package

```
headless_driver.so     compiled Linux plugin (32-bit)
headless_driver.inc    Pawn include — the only API surface you script against
example.pwn            example gamemode usage
LICENSE.txt            usage terms — read this, especially the credit requirement
```

A `headless_driver.dll` for Windows servers is not included in this package; it is built from the
same source with MSVC/CMake (`-A Win32`) and can be provided separately on request.

## Credit requirement

Using this plugin requires crediting **Loki** as its author. The plugin itself sends every
connecting player a short one-line message crediting the Headless Driver plugin — this message is
part of the plugin and must not be removed, hidden, or patched out. See `LICENSE.txt` section 2
for the full terms.

## Install

1. Copy `headless_driver.so` (or `.dll` on Windows) into your server's `plugins/` folder.
2. Add `headless_driver` to the `plugins` line in `server.cfg` (SA-MP) or `pawn.legacy_plugins`
   in `config.json` (open.mp).
3. Copy `headless_driver.inc` into `pawno/include/` (or `qawno/include/`).

## Quick start: ambient traffic

Put a road graph in `scriptfiles/` (the default is the samp-gps-plugin's `GPS.dat`; a
`carpaths.json` also works if you don't use that plugin), then:

```pawn
#include <headless_driver>

public OnGameModeInit()
{
    HD_InitTraffic();   // loads GPS.dat / carpaths.json
    return 1;
}
```

Cars will appear ~80–270 m from each player, drive on the right-hand lane, slow for corners and
other traffic, give way to player-driven cars, and despawn when nobody is near.

## Quick start: a single scripted driver

```pawn
new gCar = CreateVehicle(410, 2000.0, -1700.0, 13.5, 0.0, -1, -1, -1);
new gDriver = HD_Spawn(gCar, 20.0);
HD_DriveRoute(gDriver, routeX, routeY, routeZ, routeLen);
HD_SetLoop(gDriver, true);
```

See `example.pwn` and the comments in `headless_driver.inc` for the full native list and the
callbacks you can forward: `OnHeadlessDriverFinish`, `OnHeadlessDriverStuck`,
`OnTrafficVehicleSpawn`, `OnTrafficVehicleDespawn`.

## Support / source access

This package ships binaries only. For bug reports, feature requests, a Windows `.dll` build, or
anything needing source access, contact the author, Loki, directly.
