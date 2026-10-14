# EarShare — User Guide

Watch one movie on one Mac, with everyone listening on their own Bluetooth headphones.

## Install

**With Homebrew:**

```sh
brew install --cask tauruslabsofficial-tech/tap/earshare
```

**Or with the disk image:** open `EarShare.dmg`, drag **EarShare** onto **Applications**, then open it from Applications.

The first time you open a DMG install, macOS may say it can't verify the app. EarShare is free, but not yet notarized by Apple. Allow it once:

- **macOS 15 or later:** click **Done** on the warning. Open **System Settings → Privacy & Security**, scroll down to *“EarShare was blocked…”*, click **Open Anyway** and confirm.
- **macOS 13–14:** right-click the app → **Open** → **Open**.

## Watch something together

1. **Connect every pair of headphones** in **System Settings → Bluetooth** (the app has a button for this). They appear in EarShare as soon as they connect.
2. **Tick** the headphones that should play. You need at least two.
3. Press **Start Sharing**.
4. Play anything. Every ticked headphone hears it.
5. Press **Stop Sharing** when you're done. The Mac switches back to its previous output.

Everything is also in the **headphones icon in the menu bar**. Closing the window keeps sharing running. **Quit** stops sharing and restores your normal output.

## Volume

- Each headphone has its own **volume slider** and **mute** button. Everyone can also use the buttons on their own headphones.
- The Mac's keyboard volume keys don't control a shared output (a macOS rule). Use the sliders or the menu bar.

## Lag

EarShare adds **no delay**. Each pair of headphones sounds exactly as it does when it's the only one connected. Bluetooth headphones always have a small built-in delay of their own, and different models can differ from each other slightly. That's normal Bluetooth behaviour, not something the app adds.

## Troubleshooting

| Problem | Fix |
|---|---|
| A headphone isn't listed | Make sure it's *connected* (not just paired) in Bluetooth settings. |
| Sound only in one pair | Check it's ticked and not muted. Try Stop → Start Sharing. |
| AirPods jump to your iPhone | Bluetooth settings → ⓘ next to the AirPods → **Connect to This Mac: When Last Connected to This Mac**. |
| Crackles with 3+ headphones | The Mac's Bluetooth radio is running out of bandwidth. Sit closer, turn off other Bluetooth devices, or use two pairs. |
| One pair drops out | Sharing continues on the others, and the pair rejoins automatically when it reconnects. |
| Sound in the speakers after waking the Mac | Wait a few seconds for the headphones to reconnect. Sharing rebuilds itself. |
| Video looks slightly ahead of the sound | See *Audio–video sync* in COMPATIBILITY_AND_LIMITATIONS.md. VLC and IINA have an audio-delay setting. |
| The app crashed | Open it again. It removes the leftover output and restores your normal sound. |

## Uninstall

```sh
brew uninstall --cask earshare
```

…or quit EarShare and drag it from Applications to the Trash. Nothing else is installed.
