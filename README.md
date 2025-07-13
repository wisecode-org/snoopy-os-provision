# Debian Preseed ISO Builder for Snoopy Server

This repo contains a fully automated pipeline to build a custom hybrid (BIOS+UEFI) Debian installation ISO with an embedded preseed.cfg for unattended server installations. The resulting ISO file is saved as an artifact in GitHub Actions after the CI/CD pipeline runs.

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
