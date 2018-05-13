# Terraform Installer - Automated Terraform Installation

## Automatically Download, Extract and Install Latest or Specific Version

[![release](https://img.shields.io/github/release/robertpeteuil/terraform-installer.svg?colorB=2067b8)](https://github.com/robertpeteuil/terraform-installer)
[![bash](https://img.shields.io/badge/language-bash-89e051.svg?style=flat-square)](https://github.com/robertpeteuil/terraform-installer)
[![license](https://img.shields.io/github/license/robertpeteuil/terraform-installer.svg?colorB=2067b8)](https://github.com/robertpeteuil/terraform-installer)

---

The **terraform-install** script automates the process of downloading and installing Terraform.  It provides an ideal method for installing on new hosts, installing updates and downgrading if necessary.

This installer is similar to my [Packer Installer](https://github.com/robertpeteuil/packer-installer)

This script detects the latest version, OS and CPU-Architecture and allows installation to local or system locations.  Optional parameters allow installing a specific version and installing to /usr/local/bin without prompting.

Options:

- `-i VERSION`:  Install specific version
- `-a`:          Automatically use `sudo` to install to /usr/local/bin
  - allows for unattended installation via scripts or CD tools
  - can be set as default behavior by uncommenting line 12 (`sudoInstall=true`)
  - sudo password may be required unless NOPASSWD is enabled
- `-h`:          help
- `-v`:          display version

### Installation using this Installer

Download the installer with `curl` or `wget` and make executable

``` shell
curl -LO https://raw.github.com/robertpeteuil/terraform-installer/master/terraform-install.sh
# OR wget https://raw.github.com/robertpeteuil/terraform-installer/master/terraform-install.sh
chmod +x terraform-install.sh
```

Run the installer

``` shell
./terraform-install.sh
```

Examples with Optional Parameters

``` shell
# -i = Install specific version
./terraform-install.sh -i 0.11.1

# -a = Automatic sudo install to /usr/local/bin/  (no user prompt)
./terraform-install.sh -a
```

### System Requirements

- System with Bash Shell (Linux, macOS, Windows Subsystem for Linux)
- `unzip` - terraform downloads are in zip format
- `curl` or `wget` - script will use either one to retrieve metadata and download

Optional

- `jq` - if installed, latest version parsed from hashicorp downloads
  - Useful if latest github release differs from version on hashicorp downloads
  - Avoids github api limit of 60 requests per hour (unauthenticated)

### Script Process Details

- Determines Version to Download and Install
  - Uses Version specified by `-i VERSION` parameter (if specified)
  - Otherwise determines Latest Version
    - If `jq` installed parse version from hashicorp downloads
    - Otherwise use GitHub API to retrieve latest version
- Calculates Download URL based on Version, OS and CPU-Architecture
- Verifies URL Validity before Downloading in Case:
  - VERSION incorrectly specified with `-i`
  - Download URL Format Changed on terraform Website
- Determines Install Destination
  - Performed before Download/Install Process in case user selects `abort`
- Installation Process
  - Download, Extract, Install, Cleanup and Display Results

#### CPU Architecture Detection

CPU architecture is detected for each OS accordingly:

- Linux / Windows (WSL since this is a Bash script)
  - detected with `lscpu` or by inspecting `/proc/cpuinfo`
- macOS - uses Default Arch `amd64` as it's the only version available on macOS
- Default Value - `amd64`

### License

Apache 2.0 License - Copyright (c) 2018    Robert Peteuil
