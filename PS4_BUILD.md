# PS4 RetroArch Build Instructions

## Build Configuration

PS4 RetroBox uses RetroArch compiled with plain DRM video driver for direct framebuffer output on PS4 hardware.

### Dependencies (Ubuntu 22.04/24.04)

```bash
apt-get install -y \
    libdrm-dev libgbm-dev libegl-dev libgles-dev libudev-dev \
    libasound2-dev libpulse-dev libfreetype-dev libfontconfig-dev \
    libxkbcommon-dev libwayland-dev libx11-xcb-dev libxcb1-dev \
    libxcb-xkb-dev libxkbcommon-x11-dev libxrandr-dev libxinerama-dev \
    libxi-dev libxcursor-dev libxss-dev libssl-dev libsdl2-dev \
    nasm git liblzma-dev build-essential cmake pkg-config
```

### Build

```bash
git clone https://github.com/danyboy666/RetroArch.git
cd RetroArch
./configure \
    --enable-plain_drm \
    --enable-kms \
    --enable-egl \
    --enable-sdl2 \
    --enable-alsa \
    --enable-udev \
    --enable-freetype \
    --enable-ssl \
    --enable-opengl \
    --disable-qt \
    --disable-ffmpeg \
    --disable-opengl_core
make -j$(nproc)
```

### Install

```bash
cp retroarch /usr/bin/retroarch
chmod +x /usr/bin/retroarch
```

### PS4-Specific Notes

- PS4 amdgpu driver does not support atomic modesetting
- `MESA_LOADER_DRIVER_OVERRIDE=swrast` required for software rendering
- ES uses SDL2 framebuffer directly (no X11 needed)
- RetroArch uses GL+KMS context with software Mesa rendering
- 437 official autoconfig profiles for multi-controller support
