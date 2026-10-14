# Compatibility & Limitations

## Requirements

| | |
|---|---|
| macOS | 13 Ventura or later. Liquid Glass UI on macOS 26+, translucent material UI on 13–25 |
| Mac | Apple Silicon and Intel (universal binary) |
| Headphones | Any Bluetooth headphones or speakers macOS can use as a sound output |
| Extra software | None. No drivers, extensions or virtual audio devices |
| Permissions | None |

## How it works

EarShare automates the mechanism behind Audio MIDI Setup's **Multi-Output Device**, which is the approach that reliably feeds several Bluetooth headphones on macOS:

- It creates a public **stacked aggregate device** containing the selected headphones (`AudioHardwareCreateAggregateDevice`) and makes it the system output. Every app plays into it as usual.
- The first headphone is the **clock**. Every other headphone gets the HAL's **drift compensation** at maximum quality, which absorbs the tiny speed differences between headphone clocks over a long movie.
- It is **output-only**, so headset microphones are never opened. An open mic would drop AirPods into low-quality call mode.
- It **recovers by itself**: headphones that disconnect are dropped and re-added when they come back. If macOS auto-switches the output when AirPods reconnect, the share takes it back. It's re-checked after sleep/wake. Leftover devices from a crash are cleaned up at the next launch, and quitting restores your previous output.

### Alternatives considered

- *Capturing system audio and re-playing it* (process taps, ScreenCaptureKit): adds buffering latency, needs a recording permission, and can be blocked by streaming DRM.
- *Virtual drivers* (BlackHole, Soundflower, Loopback): need an admin-installed plug-in and add a buffer.
- *Bluetooth multipoint, AirPods Audio Sharing, Auracast*: multipoint connects one headset to two sources, which is the opposite of this use case. Audio Sharing is iPhone/iPad-only and limited to AirPods/Beats. Auracast isn't available as a macOS output.

## Latency

- **EarShare adds no latency.** In testing, audio arrived through the shared device 2.2 ms after it was sent. That's the timing offset between two independent device clocks, not added buffering.
- **Bluetooth itself has latency**, typically 100–250 ms with AAC/SBC. It comes from the codec and the headphones' own buffering. It's the same as using that headphone normally, and software can't remove it.
- **Different models differ.** If one brand is 40 ms slower than another, the two will be about 40 ms apart. Most people don't notice small differences between two different people's ears.

## Audio–video sync

Video players delay the picture to match the output's reported latency. A Multi-Output device reports the clock headphone's device latency, and in testing it did not include the per-stream latency, so players may under-compensate. The picture may then run slightly ahead of the sound. This affects every Multi-Output Device on macOS. It hasn't been measured with real Bluetooth headphones yet. VLC and IINA have an audio-delay setting that corrects it.

## Bluetooth limits

- **Two headphones** is the reliable case. **Three or more** works in the app, but the Mac's single Bluetooth radio has to carry several high-quality streams, so dropouts depend on the Mac model and interference.
- **Codecs:** macOS uses AAC (AirPods and most premium headphones) or SBC. LDAC/aptX aren't supported by macOS.
- **Sample rates:** where headphones use different rates (44.1 vs 48 kHz), EarShare matches them if the hardware allows. Otherwise the HAL resamples.
- **Headset (call) mode:** if an app uses a headphone's microphone (FaceTime, Zoom, Siri), that headphone switches to low-quality mono audio. Don't use a Bluetooth headset as the Mac's input while sharing.
- **AirPods automatic switching:** a nearby iPhone/iPad can take AirPods away. Set *Connect to This Mac → When Last Connected to This Mac*.

## macOS behaviour while sharing

- The **keyboard volume keys don't work** with a combined output. Use the per-headphone sliders, the menu bar, or the headphones' own buttons.
- A device named **“EarShare”** appears in Sound settings while sharing. Choosing another output there yourself stops sharing.
- **DRM content** (Netflix, Apple TV+, Disney+) plays normally. Nothing is captured or recorded.
- Devices whose driver forbids being the system output (some virtual/loopback drivers) can't be shared and are hidden.

## Installation, updates and signing

- EarShare is **free** but not yet **notarized** by Apple (that needs a paid Apple Developer ID). If you install from the DMG, macOS asks you to allow it once (*System Settings → Privacy & Security → Open Anyway*). The Homebrew cask clears the download quarantine for you, so that prompt doesn't appear.
- **Updates are automatic.** EarShare checks for a new version once a day and offers to install it (*EarShare → Check for Updates…* checks right away). Every update is signed with Taurus Labs' private EdDSA key, and EarShare refuses any download that doesn't match that signature.
- Homebrew's official `homebrew/cask` only accepts notarized apps, so EarShare is distributed through the Taurus Labs tap (`tauruslabsofficial-tech/homebrew-tap`).
- The app isn't sandboxed, because it creates a system-wide output device and changes the default output, so it isn't distributed through the Mac App Store.
