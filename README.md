# System76 Bluefin

This repository builds a custom, immutable Linux operating system tailored specifically for System76 hardware. 

Powered by **BlueBuild** and GitHub Actions, it takes the standard Project Bluefin Developer Experience (Nvidia) image and seamlessly bakes System76's performance tools directly into the read-only root filesystem. It is designed to provide Pop!_OS-level hardware integration on a zero-maintenance, atomic base tailored for heavy container workloads, local API development, and 3D visualization.

### What This Build Does

* **The Foundation:** Uses `bluefin-dx-nvidia` as the core OS, providing out-of-the-box support for proprietary Nvidia drivers, Distrobox, Podman, and Docker.
* **The Hardware Stack:** Pulls from reliable Fedora COPR repositories (`szydell` and `kylegospo`) to natively compile and inject `system76-firmware`, `system76-driver`, and `firmware-manager` into the immutable image.
* **Peak Performance:** Masks default Fedora power profiles and enables `com.system76.PowerDaemon` and `com.system76.Scheduler`. This ensures flawless hybrid graphics switching and aggressive foreground CPU task prioritization.
* **Cloud-Native Updates:** GitHub Actions automatically rebuilds this OS image daily. The laptop silently pulls the newly compiled OCI image in the background and applies it on the next reboot, resulting in an indestructible, constantly updated workstation.

---

# BlueBuild Template &nbsp; [![bluebuild build badge](https://github.com/blue-build/template/actions/workflows/build.yml/badge.svg)](https://github.com/blue-build/template/actions/workflows/build.yml)

See the [BlueBuild docs](https://blue-build.org/how-to/setup/) for quick setup instructions for setting up your own repository based on this template.

After setup, it is recommended you update this README to describe your custom image.

## Installation

> [!WARNING]  
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable), try at your own discretion.

To rebase an existing atomic Fedora installation to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/blue-build/template:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/blue-build/template:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

## ISO

If build on Fedora Atomic, you can generate an offline ISO with the instructions available [here](https://blue-build.org/how-to/generate-iso/#_top). These ISOs cannot unfortunately be distributed on GitHub for free due to large sizes, so for public projects something else has to be used for hosting.

## Verification

These images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). You can verify the signature by downloading the `cosign.pub` file from this repo and running the following command:

```bash
cosign verify --key cosign.pub ghcr.io/blue-build/template
```
