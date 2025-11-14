# BetterCrewLink - Anti Mic Spammer (AMS) Edition

## :arrow_down_small: Download Link :arrow_down_small:

Installer Link: https://github.com/tejashah88/BetterCrewLink-AMS/releases/download/3.1.4-E2/Better-CrewLink.Setup.3.1.4-E2_20251114.exe

## Introduction

This is a modded version of BetterCrewLink by OhMyGuus that adds volume thresholding: any loud voices that exceed a volume limit are softened while normal voices are unaffected. A decent solution if you despise people yelling into their mics and numbing your ears. If you just want the regular version of BetterCrewLink, please go here: https://github.com/OhMyGuus/BetterCrewLink.

To use it, go to Settings (the :gear: icon on top-left), scroll down to "Volume Threshold \[dB\]" and enable it. The slider adjusts the maximum incoming volume from the other players in decibels \[dB\]. As a note, this setting is subject to your PC's volume setting, so double check that before adjusting the slider.

![Settings preview for volume thresholding](docs/settings-preview.png)

## How does it work

See this repository for more information: https://github.com/tejashah88/voice-chat-volume-norm

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
