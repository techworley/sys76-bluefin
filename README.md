# sys76-bluefin

[![build-ublue](https://github.com/techworley/sys76-bluefin/actions/workflows/build.yml/badge.svg)](https://github.com/techworley/sys76-bluefin/actions/workflows/build.yml)

This repository builds a custom, immutable Linux operating system tailored specifically for System76 hardware. 

Powered by **BlueBuild** and GitHub Actions, it takes the standard Project Bluefin Developer Experience (Nvidia) image and seamlessly bakes System76's performance tools directly into the read-only root filesystem. It is designed to provide Pop!_OS-level hardware integration on a zero-maintenance, atomic base tailored for heavy container workloads, local development, and 3D visualization.

## 📥 Installation

### Option 1: Rebase from an Existing Atomic Fedora/Bluefin Install
If you are already running an rpm-ostree based system, you can instantly swap to this custom OS by running this command in your terminal:

```bash
rpm-ostree rebase ostree-unverified-registry:ghcr.io/techworley/system76-bluefin:latest

```

Reboot your machine to apply the changes.

### Option 2: Build a Bootable ISO (For Fresh Installs or VMs)

To generate a `.iso` file for a clean installation via USB or to test safely inside GNOME Boxes, use the BlueBuild CLI on your current Linux machine:

```bash
sudo bluebuild generate-iso --iso-name system76-bluefin.iso image ghcr.io/techworley/system76-bluefin:latest

```

## ⚙️ What This Build Does

* **The Foundation:** Uses `bluefin-dx-nvidia` as the core OS, providing out-of-the-box support for proprietary Nvidia drivers, Distrobox, Podman, and Docker.
* **The Hardware Stack:** Pulls from reliable Fedora COPR repositories (`szydell` and `kylegospo`) to natively compile and inject `system76-firmware`, `system76-driver`, and `firmware-manager` into the immutable image.
* **Peak Performance:** Masks default Fedora power profiles and enables `com.system76.PowerDaemon` and `com.system76.Scheduler`. This ensures flawless hybrid graphics switching and aggressive foreground CPU task prioritization.
* **Cloud-Native Updates:** GitHub Actions automatically rebuilds this OS image daily. The laptop silently pulls the newly compiled OCI image in the background and applies it on the next reboot, resulting in an indestructible, constantly updated workstation.

## Verification

To verify that the System76 scheduler is actively running and managing your CPU after installation, run:

```bash
systemctl status com.system76.Scheduler.service

```
