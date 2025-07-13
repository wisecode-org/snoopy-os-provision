# Snoopy OS Preseed ISO Builder

This repo contains a fully automated pipeline to build a **custom Debian installation ISO** with an embedded `preseed.cfg` for unattended installations on servers.  
It's includes a ready-to-use **CI/CD pipeline** configured with **GitHub Actions** that produces a hybrid (BIOS+UEFI) bootable ISO and saves it as a downloadable artifact.

The goal is to enable fast, consistent, and repeatable Debian installations with predefined users, SSH keys, static network configuration, and a secure partitioning scheme.  
No manual intervention is required during installation, just boot from the generated ISO and let it do the work.

## What this repository provides

A `preseed.cfg` file defining locale, users, partitions, SSH keys, etc.  
A GitHub Actions pipeline (`.github/workflows/debian-preseed.yml`) that:
- Downloads the latest Debian netinst ISO.
- Extracts its content and embeds the `preseed.cfg` directly into `initrd`.
- Patches `isolinux` configurations for improved boot behavior.
- Rebuilds the ISO using `xorriso` with proper hybrid BIOS+UEFI support.
- Uploads the resulting ISO as a build artifact.

Example SSH keys and hashed passwords for automated login.  
Fully documented and reproducible.

## How to build the ISO

This is handled automatically by GitHub Actions.  
Every push to the `main` branch triggers the pipeline:

1. Go to the **Actions** tab in GitHub.
2. Select the latest workflow run.
3. Download the `.iso` artifact named like:

You can also manually run the workflow if needed.

## How to create a bootable USB drive

Once you download the generated ISO artifact, you can flash it to a USB drive with the following command:

```bash
sudo dd if=debian-snoopy-preseed-YYYYMMDDHHMMSS.iso of=/dev/sdX bs=4M status=progress
sync
