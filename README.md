# deepin-ocr-plugin-manager

An OCR plugin manager and development library for UOS (UnionTech OS).

## Overview

`deepin-ocr-plugin-manager` is an optical character recognition (OCR) plugin management system. It provides unified plugin interfaces and management capabilities, including dynamic loading and management of multiple OCR plugins, hardware acceleration, and multilingual support.

## Features

- **Plugin management**: Dynamically scans, loads, and manages multiple OCR plugins.
- **Default plugin**: Includes a default OCR plugin based on PaddleOCR and NCNN.
- **Hardware acceleration**: Supports GPU/Vulkan acceleration for improved recognition performance.
- **Multilingual support**: Supports Simplified Chinese, Traditional Chinese, English, and other languages.
- **Multithreaded processing**: Supports concurrent processing to improve efficiency.
- **Flexible configuration**: Supports custom hardware settings, thread counts, and other options.

## Dependencies

### Build dependencies

- CMake (>= 3.10)
- debhelper (>= 11)
- libncnn-dev
- libopencv-mobile-dev
- pkg-config

### Runtime dependencies

- libncnn
- libopencv-mobile

## Build and install

### Build from source

```bash
mkdir build
cd build
cmake ..
make
sudo make install
```

### Build a Debian package

```bash
dpkg-buildpackage -b
```

## Usage

### Basic usage

```cpp
#include <deepinocrplugin.h>

using namespace DeepinOCRPlugin;

// Create an OCR driver instance.
DeepinOCRDriver driver;

// Load the default plugin.
if (driver.loadDefaultPlugin()) {
    // Set the image file.
    driver.setImageFile("/path/to/image.png");

    // Perform OCR.
    if (driver.analyze()) {
        // Retrieve the recognition result.
        std::string result = driver.getAllResult();
        std::vector<TextBox> boxes = driver.getTextBoxes();
    }
}
```

### Load a custom plugin

```cpp
// Get the list of available plugins.
auto plugins = driver.getPluginNames();

// Load the specified plugin.
if (driver.loadPlugin("my-custom-plugin")) {
    // Use the plugin for OCR.
    driver.analyze();
}
```

### Configure hardware acceleration

```cpp
// Enable GPU acceleration.
std::vector<std::pair<HardwareID, int>> hardware;
hardware.push_back({HardwareID::GPU_Vulkan, 0});
driver.setUseHardware(hardware);

// Set the maximum number of threads.
driver.setUseMaxThreadsCount(4);
```

### Configure languages

```cpp
// Get the list of supported languages.
auto languages = driver.getLanguageSupport();

// Set the recognition languages.
driver.setLanguage("zh-Hans_en");  // Simplified Chinese and English
```

## Project structure

```
deepin-ocr-plugin-manager/
├── assets/              # Resources, including OCR models
│   └── model/           # OCR models
├── src/                 # Source code
│   ├── deepinocrplugin.*    # Core plugin manager code
│   └── paddleocr-ncnn/      # PaddleOCR plugin implementation
├── debian/              # Debian packaging configuration
└── CMakeLists.txt       # CMake build configuration
```

## Supported plugin format

A plugin must implement the following interfaces:

- `loadPlugin()`: Loads the plugin.
- `unloadPlugin()`: Unloads the plugin.
- `pluginVersion()`: Returns the plugin version.

A plugin must provide a `libload.so` shared library and implement the corresponding OCR interfaces.

## License

This project is licensed under **LGPL-2.1-or-later**. See [LICENSE](LICENSE) for details.

The project follows the [REUSE specification](https://reuse.software/), and license information is clearly declared for all source files. The project uses the following licenses:

- **LGPL-2.1-or-later**: Main project code
- **CC0-1.0**: Debian packaging files
- **Apache-2.0**: PaddleOCR-related utility files
- **BSL-1.0**: Clipper library

Full license texts are available in the `LICENSES/` directory.

## Contributing

Issues and pull requests are welcome.

## Links

- [Project homepage](http://www.deepin.org/)
- [Issue tracker](https://github.com/linuxdeepin/deepin-ocr-plugin-manager/issues)

## Maintainer

Deepin Packages Builder <packages@deepin.com>
