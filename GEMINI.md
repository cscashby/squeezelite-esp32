# Gemini Code Assistant Context

This document provides context for the Gemini Code Assistant to understand the `squeezelite-esp32` project.

## Gemini guidance

ALWAYS before taking action, create a PLAN.md file. If that file exists and is not referenced in the request, please delete it and start another.

NEVER execute the plan without user permission. Ask for confirmation before proceeding.

Take care of escaping in C / C++ source files - ensure that the escape string sequences are not damaged while changing these files.

NEVER build the project. The user will do that after a change has been made.

## Project Overview

`squeezelite-esp32` is a firmware for ESP32-based devices that turns them into network audio players. It is primarily a client for the Logitech Media Server (LMS), but also supports streaming via Bluetooth, AirPlay, and Spotify Connect.

The project is written in C and built using the ESP-IDF framework. It is highly configurable and supports a wide range of hardware, including various DACs, displays, and input devices.

### Key Features

*   **Logitech Media Server (LMS) Client:** Play music from a local LMS server, with support for multi-room synchronized audio.
*   **Multiple Streaming Protocols:** Stream audio from Bluetooth devices, AirPlay (version 1), and Spotify (via Spotify Connect).
*   **Extensible Hardware Support:** Works with a variety of ESP32 boards and can be extended with I2S DACs, SPDIF outputs, displays, buttons, rotary encoders, IR receivers, and more.
*   **Web-based Configuration:** A web interface allows for easy configuration of the device, including Wi-Fi setup, squeezelite options, and firmware updates.
*   **Over-the-Air (OTA) Updates:** The firmware can be updated remotely through the web interface.
*   **Audio Processing:** Includes features like resampling, a 10-band equalizer, and support for various audio codecs.

## Building and Running

NEVER build the project. The user will do that after a change has been made.

## Development Conventions

*   **Code Style:** The project follows the ESP-IDF coding style.
*   **Configuration:** The primary method of configuration is through the web interface, which stores settings in Non-Volatile Storage (NVS). The `main/Kconfig.projbuild` file defines the configuration options that are available in `menuconfig`.
*   **Modularity:** The project is organized into components, which are located in the `components/` directory. Each component has its own `CMakeLists.txt` file.
*   **Hardware Abstraction:** The project uses a hardware abstraction layer to support different hardware configurations. The `targets/` directory contains code for specific hardware platforms.
