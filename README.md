# Project Name

## Description
Brief description of the project.

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)

## Installation
How to install and set up the project.

```bash
# Install the ARM toolchain.
sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
sudo apt-get update
sudo apt-get install gcc-arm-none-eabi
sudo apt-get install gdb-arm-none-eabi
```
or 
```bash
sudo apt-get install gcc-arm-none-eabi
sudo apt-get install gdb-multiarch
```

```bash
git clone https://github.com/yourusername/yourproject.git
cd yourproject
# Add installation steps here
```

## Usage
How to use the project.

```bash
echo "add-auto-load-safe-path .gdbinit" >> ~/.config/gdb/gdbinit
```
