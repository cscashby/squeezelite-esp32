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

## Bluetooth Source Components

The Bluetooth audio source functionality is primarily implemented in the following components:

*   **`components/driver_bt/bt_app_source.c`**: This file contains the core logic for the A2DP source mode. It manages the discovery of Bluetooth sinks, the connection process, and the state machine for the source mode. It also handles AVRCP commands for controlling playback on the remote sink.

*   **`components/squeezelite/output_bt.c`**: This component acts as a bridge between the Squeezelite core and the Bluetooth stack. It initializes the Bluetooth source mode, provides the audio data to be transmitted, and manages the playback state.

*   **`components/driver_bt/bt_app_core.c`**: This file provides the underlying Bluetooth task and event dispatching mechanism used by `bt_app_source.c`. It handles the low-level initialization of the Bluetooth stack.

### Bluetooth Connected Devices

The `bt_app_source.c` component maintains a list of discovered Bluetooth sink devices in a JSON array called `peers_list`. Each element in the array is a JSON object with the following properties:

*   **`name`**: The name of the Bluetooth sink device.
*   **`rssi`**: The Received Signal Strength Indication (RSSI) of the device.

This list is updated during the discovery process and can be used to display a list of available devices to the user. The `peers_list_maintain` function is responsible for managing this list, including adding, updating, and purging devices. The list is sent to the user interface via a messaging system.

## NVS (Non-Volatile Storage)

The project uses NVS to store configuration settings. The `components/platform_config/nvs_utilities.c` file provides a set of helper functions for interacting with NVS.

### Storing Arrays and Blobs

To store an array or a blob of data in NVS, you can use the `set_nvs_value_blob` function. This function takes a key, a pointer to the data, and the size of the data.

```c
// Example of storing an array of integers in NVS
int my_array[] = {1, 2, 3, 4, 5};
esp_err_t err = set_nvs_value_blob("my_array_key", my_array, sizeof(my_array));
if (err != ESP_OK) {
    // Handle error
}
```

### Reading Arrays and Blobs

To read an array or a blob of data from NVS, you can use the `get_nvs_value_blob_alloc` function. This function allocates memory for the data and reads it from NVS. It is the caller's responsibility to free the allocated memory.

```c
// Example of reading an array of integers from NVS
int *my_array = NULL;
size_t required_size = 0;
esp_err_t err = get_nvs_value_blob_alloc("my_array_key", (void **)&my_array, &required_size);
if (err == ESP_OK) {
    // Use the array
    free(my_array);
} else {
    // Handle error
}
```