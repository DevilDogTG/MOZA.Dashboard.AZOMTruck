# MOZA Dashboard: AZOM Truck

A 780x248 single-screen MOZA dashboard for the W17 wheel display in Euro Truck Simulator 2 and American Truck
Simulator: speed, cruise control, navigation speed limit, RPM, gear (crawler, DD/OD) and gearbox type, fuel and range,
oil and water temperatures, air pressure, warning lights, truck and trailer damage, axle lift, game clock and rest time.

It carries the information of the SimHub dashboard
[Trucker Information](https://github.com/DevilDogTG/SimHub.Dashboards.TruckerInfomation) in a smaller layout: the top
bar indicators, the damage section and the gear logic follow it.

![AZOM Truck](src/AZOM%20Truck/1.png)

## Requirements

- MOZA Pit House, with a wheel that has the W17 display (the dashboard targets `W17 Display`, hardware
  `RS21-W17-HW RGB-DU-V11`)
- ETS2 or ATS telemetry reaching the wheel as `v1/gameData/*` channels, through the MOZA SimHub plugin with the
  [channel mappings](#channel-mappings) below

## Channel mappings

The plugin feeds each `v1/gameData/<name>` channel from a SimHub property or formula. Built-in channels have a plugin
default; custom ones show nothing until they are mapped. Keep these in the plugin's ETS2 master defaults: a re-uploaded
`.mzdash` gets a fresh per-dashboard mapping set. A borrowed (custom) channel is numeric only, so text such as the gear
letter is worked out in the dashboard, not in a mapping.

Paths are relative to `DataCorePlugin.GameRawData.`.

| Channel | Kind | Mapping |
|---|---|---|
| `AirPressure` | built-in | `TruckValues.CurrentValues.MotorValues.BrakeValues.AirPressure` |
| `AirPressureCritical` | custom | `TruckValues.CurrentValues.DashboardValues.WarningValues.AirPressureEmergency` |
| `AirPressureWarning` | custom | `TruckValues.CurrentValues.DashboardValues.WarningValues.AirPressure` |
| `BatteryVoltageWarning` | custom | `TruckValues.CurrentValues.DashboardValues.WarningValues.BatteryVoltage` |
| `BeaconLight` | built-in | `TruckValues.CurrentValues.LightsValues.Beacon` |
| `Brake` | built-in | plugin default |
| `CruiseControlSpeedKmh` | custom | `TruckValues.CurrentValues.DashboardValues.CruiseControlSpeed.Kph` |
| `DamageCabin` | custom | `[TruckValues.CurrentValues.DamageValues.Cabin] * 100` |
| `DamageCargo` | custom | `[TrailerValues01.DamageValues.Cargo] * 100` |
| `DamageChassis` | custom | `[TruckValues.CurrentValues.DamageValues.Chassis] * 100` |
| `DamageEngine` | custom | `[TruckValues.CurrentValues.DamageValues.Engine] * 100` |
| `DamageTrailer` | custom | `[TrailerValues01.DamageValues.Body] * 100` |
| `DamageTrailerTyre` | custom | `[TrailerValues01.DamageValues.Wheels] * 100` |
| `DamageTransmission` | custom | `[TruckValues.CurrentValues.DamageValues.Transmission] * 100` |
| `DamageTyre` | custom | `[TruckValues.CurrentValues.DamageValues.WheelsAvg] * 100` |
| `DifferentialLock` | custom | `TruckValues.CurrentValues.DifferentialLock` |
| `ElectricEnabled` | custom | `TruckValues.CurrentValues.ElectricEnabled` |
| `EngineBrake` | built-in | `TruckValues.CurrentValues.MotorValues.BrakeValues.MotorBrake` |
| `ForwardGearCount` | custom | `TruckValues.ConstantsValues.MotorValues.ForwardGearCount` |
| `Fuel` | built-in | `TruckValues.CurrentValues.DashboardValues.FuelValue.Amount` |
| `FuelCapacity` | built-in | `TruckValues.ConstantsValues.CapacityValues.Fuel` |
| `FuelRange` | built-in | `TruckValues.CurrentValues.DashboardValues.FuelValue.Range` |
| `GameClockHH` | custom | `format([TimeAbsolute],'hh')` |
| `GameClockMM` | custom | `format([TimeAbsolute],'mm')` |
| `Gear` | built-in | `TruckValues.CurrentValues.DashboardValues.GearDashboards` |
| `GearRatioType` | custom | JavaScript, below |
| `GearboxTypeAutomatic` | custom | `[TruckValues.ConstantsValues.MotorValues.ShifterTypeValue]='Automatic' or [TruckValues.ConstantsValues.MotorValues.ShifterTypeValue]='Arcade'` |
| `GearboxTypeHShifter` | custom | `[TruckValues.ConstantsValues.MotorValues.ShifterTypeValue]='HShifter'` |
| `GearboxTypeSequential` | custom | `[TruckValues.ConstantsValues.MotorValues.ShifterTypeValue]='Manual'` |
| `HazardLight` | custom | `TruckValues.CurrentValues.LightsValues.HazardWarningLights` |
| `HighBeamLight` | built-in | `TruckValues.CurrentValues.LightsValues.BeamHigh` |
| `LBlinker` | built-in | `TruckValues.CurrentValues.LightsValues.BlinkerLeftOn` |
| `LowBeamLight` | built-in | `TruckValues.CurrentValues.LightsValues.BeamLow` |
| `NavigationSpeedLimit` | built-in | `NavigationValues.SpeedLimit.Kph` |
| `OilPressureWarning` | custom | `TruckValues.CurrentValues.DashboardValues.WarningValues.OilPressure` |
| `OilTemperature` | custom | `TruckValues.CurrentValues.DashboardValues.OilTemperature` |
| `OilTemperatureWarning` | custom | `[TruckValues.CurrentValues.DashboardValues.OilTemperature] > 99` |
| `ParkLight` | custom | `TruckValues.CurrentValues.LightsValues.Parking` |
| `ParkingBrake` | built-in | `TruckValues.CurrentValues.MotorValues.BrakeValues.ParkingBrake` |
| `RBlinker` | built-in | `TruckValues.CurrentValues.LightsValues.BlinkerRightOn` |
| `RestTimeHH` | custom | `format([CommonValues.NextRestStop.Time], 'hh')` |
| `RestTimeLeft` | custom | `CommonValues.NextRestStop.Minutes` |
| `RestTimeMM` | custom | `format([CommonValues.NextRestStop.Time], 'mm')` |
| `RetarderLevel` | built-in | `TruckValues.CurrentValues.MotorValues.BrakeValues.RetarderLevel` |
| `ReverseGearCount` | custom | `TruckValues.ConstantsValues.MotorValues.ReverseGearCount` |
| `Rpm` | built-in | `Drivetrain.EngineRpm` |
| `SpeedKmh` | built-in | `TruckValues.CurrentValues.DashboardValues.Speed.Kph` |
| `Throttle` | built-in | plugin default |
| `TrailerAttached` | custom | `TrailerValues01.Attached` |
| `TrailerAxleLifted` | custom | `TruckValues.CurrentValues.TrailerLiftAxleIndicator` |
| `TruckAxleLifted` | custom | `TruckValues.CurrentValues.LiftAxleIndicator` |
| `WaterTemperature` | custom | `TruckValues.CurrentValues.DashboardValues.WaterTemperature` |
| `WaterTemperatureWarning` | custom | `TruckValues.CurrentValues.DashboardValues.WarningValues.WaterTemperature` |
| `Wipers` | built-in | `TruckValues.CurrentValues.DashboardValues.Wipers` |

`GearRatioType` is 1 when gear 10 or above is direct drive (ratio 1), 2 when it is an overdrive (ratio below 1), and 0
otherwise; the dashboard shows `DD` or `OD` from it. Its mapping, in full:

```text
js:var g = $prop('DataCorePlugin.GameRawData.TruckValues.CurrentValues.DashboardValues.GearDashboards'); if (!(g >= 10)) return 0; var r = $prop('DataCorePlugin.GameRawData.TruckValues.ConstantsValues.MotorValues.GearRatiosForward' + g); return r == 1 ? 1 : (r < 1 ? 2 : 0);
```

## Layout

| Path | What it is |
|---|---|
| `src/AZOM Truck/AZOM Truck.mzdash` | The dashboard: pretty-printed JSON, so it diffs as text |
| `src/AZOM Truck/1.png` | The preview image Pit House shows |
| `src/AZOM Truck/Resource/MD5/` | Every image the dashboard uses, named by the MD5 of its content |

Release `.mzdash` files are not tracked; they are attached to GitHub releases.

## Editing with Dashboard Studio

Edit in MOZA Dashboard Studio, save so that `src/AZOM Truck/` holds the result, and commit from the repo.

- Studio copies each imported image into `Resource/MD5/` under its hash, so changing an image adds a new file.
  Delete the old file only once no `MD5/...` entry in the `.mzdash` `imageResources` list names it.
- `lastModified` in the `.mzdash` changes on every save, so expect that line in every diff.

Unlike the SimHub dashboard repos there is no link script: Pit House's installed copy
(`%LOCALAPPDATA%\MOZA Pit House\_dashes\<id>\AZOM Truck\`) does not keep the `Resource/` folder, so it cannot simply be
pointed at this repo.

## Releasing

Every PR that bumps the version is released once it merges. PRs without a bump are not released.

The version is the `"version"` field at the end of `AZOM Truck.mzdash`, `X.Y.Z`, bumped in the PR that makes the change:

| Bump | When |
|---|---|
| X | A deliberate major release, decided by the maintainer |
| Y (Z resets to 0) | A new feature, or a dependency update: Pit House or the wheel display firmware |
| Z | A fix to something that already exists |
| None | Hidden elements, refactors with no visible change, repo tooling |

The release file is the `.mzdash` on its own: Pit House and the MOZA SimHub plugin both upload a bare `.mzdash`, not
a zip, the same way MOZA ships its built-in dashboards. Copy it from a clean checkout of the merged commit rather than
exporting it from Studio, so it is byte-for-byte what is tracked (a PowerShell `>` redirect of `git show` would
re-encode it):

```powershell
git switch --detach <commit>
git status --short   # must print nothing
Copy-Item "src/AZOM Truck/AZOM Truck.mzdash" "AZOM.Truck.v<version>.mzdash"
git switch main
```

Tag that commit and publish the release with the file attached:

```powershell
git tag -a v<version> <commit> -m "release: v<version>"
git push origin v<version>
gh release create v<version> --title "v<version>" --notes-file <notes.md> --verify-tag "AZOM.Truck.v<version>.mzdash"
```

The release notes state the Pit House version the dashboard was saved with.
