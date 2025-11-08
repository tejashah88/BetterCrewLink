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

### Stage 1: Dynamics Compression
Stage 1 is to split the frequencies between low, mid, and high band. Isolating the voice from the noise is necessary since some make-up gain will be applied from the dynamics compressor. The low and high frequency noise is passed through as-is while the mid-band frequency has the dynamics compressor applied. An analyzer is also attached to it to calculate the incoming absolute volume necessary for stage 2.

### Stage 2: Hard Limitter Gain
The incoming absolute volume from stage 1 is used to apply a hard limit gain to ensure the incoming volume never exceeds a decibel amount.

See this repository for more information: https://github.com/tejashah88/voice-chat-volume-norm

### Processing Overview
1. Split voice signal into 3 bands:
    - Low-band frequency: \[0, 300 Hz\]
    - Mid-band frequency: \[300 Hz, 3,000 Hz\]
    - High-band frequency: \[3000 Hz, inf\)
2. Let low and high band frequency pass through to avoid amplifying noise via DynamicsCompressor's makeup gain
3. Apply dynamics compressor against mid-band frequency, then add compressor analyzer to determine absolute volume for hard limiter gain
4. Feed each band through gain boosting (currently all set to 1.0)
5. Calculate absolute RMS volume for mid-band channel
6. Set hard limit gain such that the output volume does not exceed the defined


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

