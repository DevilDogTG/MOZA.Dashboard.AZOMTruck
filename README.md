# MOZA Dashboard: AZOM Truck

A 780x248 single-screen MOZA dashboard for the W17 wheel display in Euro Truck Simulator 2 and American Truck
Simulator: speed, cruise control, navigation speed limit, RPM, gear and gearbox type, fuel and range, oil and water
temperatures, air pressure, warning lights, truck and trailer damage, axle lift, game clock and rest time.

![AZOM Truck](src/AZOM%20Truck/1.png)

## Requirements

- MOZA Pit House, with a wheel that has the W17 display (the dashboard targets `W17 Display`, hardware
  `RS21-W17-HW RGB-DU-V11`)
- ETS2 or ATS telemetry reaching Pit House as `v1/gameData/*` properties

## Layout

| Path | What it is |
|---|---|
| `src/AZOM Truck/AZOM Truck.mzdash` | The dashboard: pretty-printed JSON, so it diffs as text |
| `src/AZOM Truck/1.png` | The preview image Pit House shows |
| `src/AZOM Truck/Resource/MD5/` | Every image the dashboard uses, named by the MD5 of its content |

Release files are not tracked; they are attached to GitHub releases.

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

Build the release file from the merged commit rather than exporting it from Studio, so it holds exactly the files
tracked under `src/`. Pit House's import format is not confirmed yet: settle it with a test import before the first
release. If it takes a zip of the dashboard folder:

```powershell
git archive --format=zip -9 -o "AZOM.Truck.v<version>.zip" <commit>:src
```

Tag that commit and publish the release with the file attached:

```powershell
git tag -a v<version> <commit> -m "release: v<version>"
git push origin v<version>
gh release create v<version> --title "v<version>" --notes-file <notes.md> --verify-tag "AZOM.Truck.v<version>.zip"
```

The release notes state the Pit House version the dashboard was saved with.
