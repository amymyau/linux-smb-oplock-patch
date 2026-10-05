# Linux Kernel SMB Subsystem: Oplock Patch & Validation

Experimental patch for the in-kernel SMB server (`fs/smb/server/oplock.c`) focused on refining opportunistic lock handling and concurrency control.

## Overview
This repository documents an upstream Linux kernel contribution workflow, featuring cross-compilation toolchains, QEMU emulation scripts, and protocol-level testing setup.

## Project Structure
```text
├── patches/
│   └── smb-oplock-fix.patch      # Local patch generated via git diff
├── scripts/
│   └── build-emulate.sh          # Cross-compilation and QEMU run scripts
└── README.md

# linux-smb-oplock-patch
linux-smb-oplock-patch

Environment & Toolchain
Host: WSL2 / Bare-metal Raspberry Pi CM4

Target Architecture: aarch64

Cross-Compiler: aarch64-linux-gnu-gcc

Emulation Target: qemu-system-aarch64 (-M virt)

Build & Test Workflow
Cross-Compile Kernel:

Bash
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- defconfig
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- -j$(nproc)
Run in QEMU:

Bash
qemu-system-aarch64 -M virt -cpu cortex-a57 -smp 2 -m 1G \
    -kernel arch/arm64/boot/Image \
    -append "root=/dev/vda rw console=ttyAMA0" \
    -drive if=none,file=rootfs.ext4,id=hd0 -device virtio-blk-device,drive=hd0 \
    -netdev user,id=net0 -device virtio-net-device,netdev=net0 \
    -nographic
