# BetterCrewLink - Anti Mic Spammer Edition (Beta)

## :arrow_down_small: Download Link :arrow_down_small:

Installer Link: https://github.com/tejashah88/BetterCrewLink/releases/download/v3.1.4-E/Better-CrewLink.Setup.3.1.4-E_20251108.exe

## Introduction

This is a modded version of BetterCrewLink by OhMyGuus that adds voice volume normalization: it softens loud voices (to protect against mic spammers) and (sometimes) boosts quiet voices (for the shy people in the back). If you just want the regular version of BetterCrewLink, please go here: https://github.com/OhMyGuus/BetterCrewLink.

To use it, go to Settings (the :gear: icon on top-left), scroll down to "Player Volume Tweaks" and enable it. The slider adjusts the maximum incoming volume from the other players in decibels (dB). Refer to the chart below for a [quick conversion chart](#decibel-to-volume-conversion-chart) between decibels and volume amount.

![Settings preview for volume normalization](docs/settings-preview.png)

## Decibel to Volume Conversion Chart

| Decibels (dB) | Volume (%) |
| ------------: | ---------: |
|             0 |     100.0% |
|            -6 |      50.0% |
|           -12 |      25.0% |
|           -18 |      12.5% |
|           -24 |       6.3% |
|           -30 |       3.2% |
|           -36 |       1.6% |
|           -42 |       0.8% |
|           -48 |       0.4% |
|           -54 |       0.2% |
|           -60 |       0.1% |

## How does it work

### Stage 1: Multi-Band Compression
Stage 1 splits the frequencies between low, mid, and high bands. Isolating the voice from the noise is necessary since makeup gain will be applied from the dynamics compressor. The low and high frequency noise is passed through as-is while the mid-band frequency has the dynamics compressor applied to normalize voice volume.

### Stage 2: Band Mixing
After processing, the three bands are automatically mixed back together by the Web Audio API, combining the compressed mid-band (with makeup gain) with the uncompressed low and high bands at their original levels.

### Stage 3: Two-Stage Limiter
The system uses a hybrid two-stage limiting architecture to ensure the volume never exceeds the threshold:

1. A secondary dynamics compressor ratio provides smooth volume limiting for all combined bands. This handles most of the limiting work smoothly without audible artifacts like volume level jumps.
2. Finally, the overall audio signal is hard gain limited by the user's desired volume decibel threshold. This is done with calculating the RMS, converting to decibels and adjusting the gain dynamically. This can potentially produce artifacting but the secondary dynamics compressor is meant to act as a buffer.

See this repository for more information: https://github.com/tejashah88/voice-chat-volume-norm

### Processing Overview
1. Split voice signal into 3 bands:
    - Low-band frequency: [0, 300 Hz]
    - Mid-band frequency: [300 Hz, 3,000 Hz]
    - High-band frequency: [3000 Hz, inf)
2. Let low and high band frequency pass through to avoid amplifying noise via DynamicsCompressor's makeup gain
3. Apply dynamics compressor against mid-band frequency for voice normalization
4. Mix all three bands back together (low + compressed mid + high)
5. Apply first-stage limiter (DynamicsCompressor with 20:1 ratio) for smooth limiting at audio rate
6. Measure signal with analyzer after first-stage limiter
7. Apply safety limiter (GainNode) to enforce true hard ceiling, ensuring output never exceeds the absolute volume threshold.


## Developer Notes
To work on this project, you'll need the latest version of Node 16, since Electron depends on it. I recommend either using nvm (node version manager) or using Docker or a VM setup (what I did).

```bash
# Install dependencies
npm install -g yarn
yarn install

# Build for Windows & Linux respectively
yarn run dist
yarn run dist:linux
```

