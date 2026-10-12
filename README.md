# Pokesurvivor for Android

Three Android WebView apps for playing [Pokesurvivor](https://pokesurvivor.com/) with a gamepad, keyboard or mouse. All include a custom adaptive launcher icon, analog movement, menu navigation and camera zoom. 

## Choose a version

| | Manual | Auto | Homing |
|---|---|---|---|
| Home-screen name | Pokesurvivor | Pokesurvivor Auto | Pokesurvivor Homing |
| APK filename | `Pokesurvivor.apk` | `Pokesurvivor-Auto.apk` | `Pokesurvivor-Homing.apk` |
| Source directory | [`manual/`](manual/) | [`auto/`](auto/) | [`homing/`](homing/) |
| Android package | `com.pokesurvivor.wrapper` | `com.pokesurvivor.auto` | `com.pokesurvivor.homing` |
| Firing | Hold RT to fire | Fires automatically in your aim direction | Normal automatic enemy targeting and attacks |

All versions preserve each move's normal cooldown. In Manual and Auto, centering the right stick retains your last aim direction. Homing uses the original game's automatic target selection and each move's built-in homing behavior. The apps can be installed together because they use different Android packages.

Each app has separate WebView storage. Sign in separately to use the same game account; local guest data does not automatically transfer between the apps.

## Requirements

- Android 8.0 or newer.
- Internet access to load the live game.
- A Bluetooth or USB gamepad for controller play. Keyboard and mouse input are also supported.

## Install

1. Transfer your chosen APK to the Android device and open it.
2. Allow installation from that file source when Android asks, then install the app.
3. Connect your controller and open your chosen version.
4. Sign in through the game's existing form or choose its guest option.

Tap a sign-in field to open the Android keyboard. A physical keyboard can also enter text. The app menu in the top-right has **Show keyboard**, a controls guide, reload and close options.

Compiled APKs are excluded from this repository by `.gitignore`. They can be distributed through GitHub Releases or built from source.

## Gamepad controls

| Control | Action |
|---|---|
| Left stick | Analog movement |
| Right stick | Aim in Manual/Auto; Homing aims automatically |
| RT / R2 | Hold to fire in Manual; unnecessary in Auto/Homing |
| D-pad Up | Select move slot 1 |
| D-pad Right | Select move slot 2 |
| D-pad Down | Select move slot 3 |
| D-pad Left | Select move slot 4 |
| Y | Cycle to the next move |
| R1 | Zoom in |
| L1 | Zoom out |
| Start | Pause / resume |
| L3 | Reroll upgrades |
| Select | Open the app menu |

Zoom changes in 10% steps, from 75% to 175%. Movement includes a dead zone to reduce stick drift.

In menus, use the left stick or D-pad to navigate, **A** to confirm and **B** to go back. This includes sign-in fields, personality questions, Pokémon selection, upgrades and pause. Button names follow Android's button positions; some controllers use different printed labels.

## Keyboard and mouse

| Input | Action |
|---|---|
| WASD / arrow keys | Move |
| Mouse movement | Aim in Manual/Auto; Homing targets automatically |
| Hold mouse button or Space | Fire in Manual; unnecessary in Auto/Homing |
| 1–4 | Select a move or a numbered upgrade option |
| Q / E | Previous / next move |
| R | Reroll upgrades |
| Escape / P | Pause |
| Tab / Shift+Tab | Navigate sign-in fields and other focusable controls |
| Enter | Submit the sign-in form or activate a focused control |

Typing in fields retains native Android WebView behavior and is shielded from gameplay shortcuts. The game's existing touch movement and menus remain available.

## Attack behavior

**Manual:** no new moves are cast without a firing input. Releasing RT, the mouse button or Space stops new casts.

**Auto:** moves cast automatically while playing, in the direction you are aiming. New casts stop in menus, while editing a field, or when the page is hidden. Automatic firing resumes when active gameplay resumes.

**Homing:** the original game selects enemies, aims and attacks automatically. It retargets enemies according to its own rules and preserves the existing homing settings on projectiles. It does not force every move to become a homing projectile. New casts stop in menus, while editing a field, or when the page is hidden.

In Manual and Auto, directional projectiles, beams, melee, boomerangs, dashes and cones follow your aim. All versions retain the intrinsic behavior of homing, chain, radial, ground-targeted and area moves. Existing projectiles, orbiting attacks and persistent fields continue or expire according to the game's rules.

## Build from source

Open `manual/`, `auto/` or `homing/` as an Android Studio project. Install JDK 17 or newer, Android SDK Platform 35 and the Android build tools. Configure your SDK path through Android Studio or an untracked `local.properties` file.

From the selected project directory, run:

```powershell
# Windows
.\gradlew.bat assembleDebug
```

```sh
# Linux / macOS
chmod +x gradlew
./gradlew assembleDebug
```

The APK is generated at `app/build/outputs/apk/debug/app-debug.apk` inside that project. Each version builds independently; there is no root Gradle project.

The supplied APKs are development-signed. Rebuilds normally use your own Android debug key. Android requires matching signing certificates for an in-place update; uninstall the supplied app before installing a build signed with a different key. Uninstalling clears that app's local storage. Use your own release signing key for release builds, and keep it out of Git.

## Repository layout

```text
README.md
LICENSE
.gitignore
THIRD_PARTY_NOTICES.md
manual/                 Manual firing Android project
auto/                   Automatic firing Android project
homing/                 Original enemy targeting Android project
third-party/            Gradle license and notice
```

In each project, `MainActivity.java` receives Android controller buttons and motion axes. `app/src/main/assets/controller.js` adapts movement, aim, firing, focus navigation and zoom to the live game. The wrapper does not expose a JavaScript-to-native interface. External links open in the browser.

## Validation and known limits

Java compilation, JavaScript syntax checks, control integration tests against the game's combat module, APK v2/v3 signature verification and ZIP alignment passed. Tests cover movement, aim, firing and cooldowns, move selection, cycling, zoom, text-entry isolation and menu navigation. Auto tests also cover retained aim and firing suppression in menus, during typing and while hidden. Homing tests use the original player and combat modules to verify enemy targeting, retargeting, projectile homing settings, cooldowns, no-enemy behavior and pause/background suppression.

No Android phone, emulator or physical controller was connected during development. On-device rendering, hardware mappings, keyboard behavior and actual account authentication remain unverified.

The app loads the live website and requires internet access. The adapter depends on the game's exposed `G.Input`, `G.Player`, `G.Combat` and `G.Camera` APIs; future game updates may require wrapper changes.

## Device test checklist

1. Check the launcher name and icon. Type in both sign-in fields with your Android keyboard and, if used, a physical keyboard.
2. Start a run. Test left-stick movement and confirm movement stops when the stick is released.
3. In Manual, aim and hold RT to fire, then release it to stop new casts. In Auto, verify firing without RT and retained aim when the right stick is centered. In Homing, verify automatic targeting and retargeting without right-stick or trigger input.
4. Test all D-pad slots, Y cycling, zoom, upgrades, menu confirmation and pause.
5. Disconnect the controller and check that movement stops. Background the app and confirm new casts stop. Resume and confirm the chosen version's firing behavior.

## License and attribution

The original wrapper code, documentation and custom launcher artwork in this repository are available under the [MIT License](LICENSE).

The live Pokesurvivor game, Pokémon content and third-party tools are not covered by that license. Game source, sprites, music and other game assets are not bundled here. This is an independent wrapper. See [third-party notices](THIRD_PARTY_NOTICES.md) for Gradle attribution and the game link.
