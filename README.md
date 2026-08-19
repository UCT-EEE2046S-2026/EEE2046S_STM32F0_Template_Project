# SISPS-PV

uUCT EEE2046S Template Project for the UCT Developemnt Board

## Overview

This is a general purpose starter repository intended for programming the UCT Development Board with minimal stoftware bloat.

## Repository Structure

This repository contains a non trivial file structure and build system. This section will outline the folder structure and build process.

## Folders

### [Inc](Inc) & [Src](Src)

Main application Include and Source directories.

## Build Process

This project uses CMake to configure the build process.

### Crosscompile Builds

Crosscompile builds are intended to produce binaries for the embedded target. These builds are configured to use specifically the ARM GCC Embedded (`arm-none-eabi-`) toolchain. The build artefacts are `.elf` and `.hex` files, which are intended to be flashed to the target.

### User Presets

[CMakePresets.json](CMakePresets.json) is intended for general-purpose use. To override these presets, create a file called `CMakeUserPresets.json` in the root of the repository. This file will be ignored by git. Use this to experiment changes and, if you wish, submit a PR to update the main presets file.

More info on `CMakeUserPresets.json` can be found [here](https://cmake.org/cmake/help/latest/manual/cmake-presets.7.html#user-presets-file).
