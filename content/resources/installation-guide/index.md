---
type: "page"
title: "Installation Guide"
subtitle: ""
draft: false
sidebar: true
Reviewed: 4 June 2024
Reviewer: Tim Sutton
---

{{< content-start  >}}

# QGIS Installers

QGIS is available on Windows, macOS, Linux, FreeBSD, OpenBSD, Android and iOS.

![Icons of Operating Systems](img/icons.png)

We recommend installing the Long Term Release (LTR) packages since our LTR versions undergo more rigorous testing before release and receive at least one year of bug fix updates.

See also [the main installers page]({{< ref "download" >}}).

To evaluate the upcoming release and to allow non-developers to support development we also provide testing packages (note the [warning](#warning)).

In the feature frozen phase preceding a release (see [Release schedule]({{< ref "roadmap#release-schedule" >}})) these packages are effectively **pre-releases, which we strongly urge users to test**.

# Windows

There are two options for installing on Windows:

## Standalone installers

Standalone installers include everything QGIS needs in a single download. Once you have the installer, no internet is required to complete the installation. When a new release is available, you need to download the complete installer again in order to upgrade. For beginners, the standlone installer is probably the easiest way to install QGIS:

|Release|Version|Package|
|---|---|---|
|Latest Release|{{< param "release" >}} {{< param "codename" >}} {{< param "releasenote" >}}|[Installer]({{< param "lr_msi" >}}) [Checksum]({{< param "lr_sha" >}})|
|Long Term Release|{{< param "ltrrelease" >}} {{< param "ltrcodename" >}} {{< param "ltrnote" >}}|[Installer]({{< param "ltr_msi">}}) [Checksum]({{< param "ltr_sha">}})|
|Development|{{< param "devversion" >}} master|[Weekly snapshots]({{< param "weekly_msi">}})|

The weekly snapshots of the nightly qgis-dev package of OSGeo4W are for users that cannot use OSGeo4W (see below) for some reason or just prefer standalone installers. In the feature freeze phase, theset also act as **release candidate** installers.

## OSGeo4W installer

More advanced QGIS users should use OSGeo4W packages. This installer makes it possible to install several versions of QGIS in parallel and also to do much more efficient updates as only changed components are downloaded and installed with each new release.

The OSGeo4W repository contains a lot of software from OSGeo projects. QGIS and all dependencies are included, along with Python, GRASS, GDAL, etc. The installer is able to install from internet or just download all needed packages beforehand. The downloaded files are kept in a local directory for future installations and could also be used to install offline.

Installation steps are:

- Download [OSGeo4W Installer](https://download.osgeo.org/osgeo4w/v2/osgeo4w-setup.exe) and run it.
    
- Choose _Express Install_ and select _QGIS_ to install the _latest release_ and/or _QGIS LTR_ to install the _long term release_.
    

Alternatively, instead of doing the _Express_ install, you can use the _Advanced Install_, navigate to the _Desktop_ section and pick one or more of the following packages:

|Release|Version|Package|Description|
|---|---|---|---|
|Latest Release|{{< param "release" >}} {{< param "codename" >}} {{< param "releasenote" >}}|qgis|Release|
|||qgis-rel-dev [[1]](#id1)|Nightly build of the upcoming point release|
|Long Term Release|{{< param "ltrrelease" >}} {{< param "ltrcodename" >}} {{< param "ltrnote" >}}|qgis-ltr|Release|
|||qgis-ltr-dev [[1]](#id1)|Nightly build of the upcoming long term point release|
|Development|{{< param "devversion" >}} master|qgis-dev [[1]](#id1)|Nightly build of the development version|

{{< footnote "1" >}} Nightlies are debug builds (including debugging output that can be used by developers to better understand issues with the build).

The packages listed in the above table only install the necessary dependencies needed to run QGIS. Corresponding to those packages there are also meta packages with the suffix `-full-free` and `-full`. The `-full-free` contains additional optional dependencies that some popular (not included in the default QGIS install) plugins use.  The `-full` includes everything from `-full-free` and also adds proprietary extensions like Oracle drivers, ECW and MrSID.

The Express installs reference the corresponding `-full` variant and the standalone installers are also made from these OSGeo4W package sets.

Before installing any of the nightly builds note the [warnings](#warning) below.

# Linux

Most linux distributions split QGIS into several packages; you’ll probably need `qgis` and `qgis-python` (to run plugins). Packages like `qgis-grass` (or `qgis-plugin-grass`), `qgis-server` can be installed when you need them.

Below you will find specific instructions per distribution. For most distro’s there are instructions to install QGIS stable and instructions to install a cutting edge QGIS testing build (note the [warning](#warning)).

## Debian / Ubuntu

### Quickstart

{{< rich-box-start icon="💁" layoutClass="tips">}}
{{< rich-content-start themeClass="coloring-1" >}}
##### Note
In the section following this one, you will find ALL possible options to install different versions of QGIS in different versions of Debian/Ubuntu. If you have problems, verify whether your distribution is still supported as the repositories also contain older **unsupported** distributions with the last QGIS version that was supported. Be aware that those might have meanwhile ceased to work.
{{< rich-content-end >}}
{{< rich-box-end >}}

Simply install the latest stable QGIS ({{< param "version" >}}.x {{< param "codename" >}}) in your Debian or Ubuntu without having to edit config files.

{{< rich-box-start icon="🌀" layoutClass="tips">}}
{{< rich-content-start themeClass="coloring-1" >}}
##### Note
Although you see ‘Debian’ in some places, this also works for ‘Ubuntu’, as one is actually a symlink to the other on our server.
{{< rich-content-end >}}
{{< rich-box-end >}}

First install some tools you will need for these instructions:

```
sudo apt install gnupg software-properties-common
```

Now install the QGIS Signing Key, so QGIS software from the QGIS repo will be trusted and installed:

```
sudo mkdir -m755 -p /etc/apt/keyrings  # not needed since apt version 2.4.0 like Debian 12 and Ubuntu 22 or newer
sudo wget -O /etc/apt/keyrings/qgis-archive-keyring.gpg https://download.qgis.org/downloads/qgis-archive-keyring.gpg
```

Add the QGIS repo for the latest stable QGIS ({{< param "version" >}}.x {{< param "codename" >}}) to `/etc/apt/sources.list.d/qgis.sources`:

```
Types: deb deb-src
URIs: https://qgis.org/debian
Suites: your-distributions-codename
Architectures: amd64
Components: main
Signed-By: /etc/apt/keyrings/qgis-archive-keyring.gpg
```

{{< rich-box-start icon="💬" layoutClass="tips">}}
{{< rich-content-start themeClass="coloring-1" >}}
##### Note
`Suites` in above lines depends on your distribution. `lsb_release -cs` will show your distribution name.

In some distributions (like Linux Mint), `. /etc/os-release; echo "$UBUNTU_CODENAME"` will show the correct distibution name.

See [Available codenames](#available-codenames).
{{< rich-content-end >}}
{{< rich-box-end >}}

Update your repository information to also reflect the newly added QGIS one:

```
sudo apt update
```

Now, install QGIS:

```
sudo apt install qgis qgis-plugin-grass
```

{{< rich-box-start icon="✍️" layoutClass="tips">}}
{{< rich-content-start themeClass="coloring-1" >}}
##### Note
Add `qgis-server` to the above line if you also want to install QGIS Server
{{< rich-content-end >}}
{{< rich-box-end >}}

### Repositories

Default Debian and Ubuntu software repositories often contain older versions of QGIS.

To have newer versions, you have to add alternative software repositories, by adding one of the deb-lines below to your `/etc/apt/sources.list.d/qgis.sources` file.

Our main repository contains multiple lines of packages for several versions of **Debian and Ubuntu** based on the dependencies the individual distributions provide.

For Ubuntu we also used to have extra packages in a separate repository that are based on [ubuntugis](https://launchpad.net/~ubuntugis), which held more up-to-date versions of other GIS packages than Ubuntu itself for LTS versions. If you want those you also need to include ubuntugis-unstable ppa in your /etc/apt/sources.list.d/qgis.list file (see [ubuntugis documentation](https://trac.osgeo.org/ubuntugis/wiki/UbuntuGISRepository)).

{{< rich-box-start icon="💁" layoutClass="tips" mode="html" >}}
{{< rich-content-start themeClass="coloring-1" >}}
##### Note
The release packages are only produced once, shortly after a new version has been released. As unstable, not yet released debian versions (testing) and ubuntugis-unstable can have library changes the packages might sooner or later be broken for these targets, when the development in debian, ubuntu or ubuntugis-unstable moves on and their packages used as dependencies in QGIS change. In that event you can either:

- move to a stable debian version and use the released packages for it,
    
- wait for the next point release (due {{< param "nextpointreleasedate" >}}), which happens every month and will include the updated dependencies,
    
- switch to a nightly repository (available for the **two** release branches and master) whose packages are rebuild on regular basis and will also pickup the updated dependencies automatically or
    
- build your own set of packages (see [build-debian-packages](https://github.com/qgis/QGIS/blob/master/INSTALL.md#310-building-debian-packages)).
{{< rich-content-end >}}
{{< rich-box-end >}}
    

Lines of packages:

| Release | Version | Description | Repository |
| ---- | ---- | ---- | ---- |
| Latest Release | {{< param "version" >}}.x {{< param "codename" >}} {{< param "releasenote">}} | Release for **Debian and Ubuntu** | https://qgis.org/debian <br> https://qgis.org/ubuntu |
||| Release with ubuntugis-unstable dependencies | https://qgis.org/ubuntugis |  |  |
||| Nightly build of upcoming point release for Debian and Ubuntu [[2]](#id2) | https://qgis.org/debian-nightly-release <br> https://qgis.org/ubuntu-nightly-release |  |  |
||| Nightly build of upcoming point release with ubuntugis-unstable dependencies [[2]](#id2) | https://qgis.org/ubuntugis-nightly-release |  |  |
| Long Term Release Repository | {{< param "ltrversion" >}}.x {{< param "ltrcodename" >}} {{< param "ltrnote" >}} | Release for **Debian and Ubuntu** | https://qgis.org/debian-ltr https://qgis.org/ubuntu-ltr |
||| Release with ubuntugis-unstable dependencies | https://qgis.org/ubuntugis-ltr |  |  |
||| Nightly build of upcoming point release for Debian and Ubuntu [[2]](#id2) | https://qgis.org/debian-nightly-ltr <br> https://qgis.org/ubuntu-nightly-ltr |  |  |
||| Nightly build of upcoming point release with ubuntugis-unstable dependencies [[2]](#id2) | https://qgis.org/ubuntugis-nightly-ltr |  |  |
| Development Version | {{< param "devversion" >}} master | Nightly build for **Debian and Ubuntu** [[2]](#id2) | https://qgis.org/debian-nightly <br> https://qgis.org/ubuntu-nightly |
||| Nightly build with ubuntugis-unstable dependencies [[2]](#id2) | https://qgis.org/ubuntugis-nightly |  |  |

{{< footnote "2" >}} Nightlies are debug builds (including debugging output)

<small>
Next point release: {{< param "nextpointreleasedate" >}}

Next release: {{< param "nextreleasedate" >}}

(For more dates see Release Schedule on [Road Map]({{< ref "resources/roadmap" >}}))
</small>

#### Supported distribution versions: {#available-codenames}

|Distribution|Version         |Codename|Also available based on ubuntugis-unstable dependencies?|
|------------|----------------|--------|--------------------------------------------------------|
|Debian      |12.x (stable)   |bookworm|                                                        |
|            |11.x (oldstable)|bullseye [[3]](#id3) |                                           |
|            |unstable        |sid     |                                                        |
|Ubuntu      |24.10           |oracular [[4]](#id4) |                                           |
|            |24.04 (LTS)     |noble   |yes                                                     |
|            |23.10           |mantic  |                                                        |
|            |22.04 (LTS)     |jammy   |yes                                                     |

{{< footnote "3" >}} only up to 3.38 (bullseye's GRASS too old)

{{< footnote "4" >}} starting with nightlies after 3.34.12/3.40.0


To use the QGIS archive you have to first add the archive’s repository public key:

```
wget https://download.qgis.org/downloads/qgis-archive-keyring.gpg
gpg --no-default-keyring --keyring ./qgis-archive-keyring.gpg --list-keys
```

Should output:

```
./qgis-archive-keyring.gpg
--------------------------
pub   rsa4096 2022-08-08 [SCEA] [expires: 2027-08-08]
      2D7E3441A707FDB3E7059441D155B8E6A419C5BE
uid           [ unknown] QGIS Archive Automatic Signing Key (2022-2027) <qgis-developer@lists.osgeo.org>
```

After you have verified the output you can install the key with:

```
sudo mkdir -m755 -p /etc/apt/keyrings  # not needed since apt version 2.4.0 like Debian 12 and Ubuntu 22 or newer
sudo cp qgis-archive-keyring.gpg /etc/apt/keyrings/qgis-archive-keyring.gpg
```

Alternatively you can download the key directly without manual verification:

```
sudo mkdir -m755 -p /etc/apt/keyrings  # not needed since apt version 2.4.0 like Debian 12 and Ubuntu 22 or newer
sudo wget -O /etc/apt/keyrings/qgis-archive-keyring.gpg https://download.qgis.org/downloads/qgis-archive-keyring.gpg
```

With the keyring in place you can add the repository as `/etc/apt/sources.list.d/qgis.sources` with following content:

```
Types: deb deb-src
URIs: *repository*
Suites: *codename*
Architectures: amd64
Components: main
Signed-By: /etc/apt/keyrings/qgis-archive-keyring.gpg
```

Example for the latest long term release for Ubuntu 22.04 Jammy:

```
Types: deb deb-src
URIs: https://qgis.org/ubuntu-ltr
Suites: jammy
Architectures: amd64
Components: main
Signed-By: /etc/apt/keyrings/qgis-archive-keyring.gpg
```

After that type the commands below to install QGIS:

```
sudo apt update
sudo apt install qgis qgis-plugin-grass
```

In case you would like to install QGIS Server, type:

```
sudo apt update
sudo apt install qgis-server --no-install-recommends --no-install-suggests
# if you want to install server Python plugins
apt install python3-qgis
```

{{< rich-box-start icon="💁" layoutClass="tips">}}
{{< rich-content-start themeClass="coloring-1" >}}
##### Note
Please remove all the QGIS and GRASS packages you may have installed from other repositories before doing the update.
{{< rich-content-end >}}
{{< rich-box-end >}}

## Fedora

Get packages for Fedora varians using `dnf` by typing:

```
sudo dnf install qgis python3-qgis qgis-grass qgis-server
```

If you are on **Fedora Atomic Desktops**, you have 2 options:

1. Install QGis to the system

This is the easiest method, saves data and disk space, but will slow down system updates a bit

```
rpm-ostree install qgis python3-qgis qgis-grass qgis-server
```

2. Install in a toolbox or distrobox

See the chapter below 

Fedora switches between the current QGis release and the LTs releases. The unstable "Rawhide" branch will contain newer but possibly buggy QGis Versions.

In case you would like to install QGIS Server (note that it’s not a common practice to install both client and server applications on the same machine), type:

```
sudo dnf install qgis-server python3-qgis
```

|Distribution|Version|QGIS version|GRASS GIS version|
|---|---|---|---|
|Fedora|40|3.34.14|3.34.14|
||41|3.40.2|3.40.2|

Always up-to-date version infos:
- [QGis](https://packages.fedoraproject.org/pkgs/qgis/qgis)
- [QGis-Grass](https://packages.fedoraproject.org/pkgs/qgis/qgis-grass)

## NixOS

Latest stable and LTR packages are [available in nixpkgs](https://search.nixos.org/packages?channel=24.05&show=qgis&from=0&size=50&sort=relevance&type=packages&query=qgis). 

### Basic Install

**Ephemeral use:**

```
nix-shell -p qgis --command qgis
``` 
or 

```
nix-shell -p qgis-ltr  --command qgis
```

**Or added to your NixOS configuration.nix so that it becomes part of your system install:**

For the LTR:

```  
  environment.systemPackages = [
    pkgs.qgis-ltr
  ];
```

For latest stable:
```
environment.systemPackages = [
    pkgs.qgis
  ];
```

### Running with extra python packages:

Because of the atomic nature of NixOS packages, you need to override the package if you want extra python packages to be available to QGIS. For example to run QGIS with numpy, geopandas and rasterio python libraries you can do:

**Ephemeral use:**

```
nix-shell -p \
      'qgis.override { extraPythonPackages = (ps: [ ps.numpy ps.future ps.geopandas ps.rasterio ps.debugpy ]);}' \
      --command "qgis"
```

**Or added to your NixOS configuration.nix so that it becomes part of your system install:**

```
{
  config,
  pkgs,
  ...
}: {
  environment.systemPackages = with pkgs; [
    (qgis.override {
      extraPythonPackages = ps:
        with ps; [
          numpy
          geopandas
          rasterio
        ];
    })
    # other packages
  ];
}

```

## SUSE / openSUSE

Latest stable and LTR packages called `qgis` and `qgis-ltr` are available in the following repositories:

|Distribution|Repository|
|---|---|
|Tumbleweed|https://download.opensuse.org/repositories/Application:/Geo/openSUSE_Tumbleweed/|
|Leap 15.2|https://download.opensuse.org/repositories/Application:/Geo/openSUSE_Leap_15.2/|
|Leap 15.1|https://download.opensuse.org/repositories/Application:/Geo/openSUSE_Leap_15.1/|
|Factory ARM|https://download.opensuse.org/repositories/Application:/Geo/openSUSE_Factory_ARM/|
|Factory PowerPC|https://download.opensuse.org/repositories/Application:/Geo/openSUSE_Factory_PowerPC/|
|SLE 15 SP1 Backports|https://download.opensuse.org/repositories/Application:/Geo/SLE_15_SP1_Backports/|
|SLE 15 SP1 Backports debug|https://download.opensuse.org/repositories/Application:/Geo/SLE_15_SP1_Backports_debug/|

All packages include GRASS and Python support.

All openSUSE Geo repositories can be found here: https://download.opensuse.org/repositories/Application:/Geo/

## Mandriva

### QGIS stable

Current:

```
urpmi qgis-python qgis-grass
```

## Slackware

### QGIS stable

Packages on https://slackbuilds.org/result/?search=qgis

## Arch Linux

### QGIS stable

QGIS stable is available in Arch Linux official repository : https://archlinux.org/packages/extra/x86_64/qgis/

Install with:

```
pacman -S qgis
```

### QGIS LTR

QGIS Long Term Release is available in AUR (Arch User Repository).

Install with yaourt or other package manager which supports AUR:

```
yaourt -S qgis-ltr
```

For bugs and other behaviour, read comments here : https://aur.archlinux.org/packages/qgis-ltr/

### QGIS testing

QGIS testing is available in AUR (Arch User Repository).

Install with yaourt or other package manager which supports AUR:

```
yaourt -S qgis-git
```

For bugs and other behaviour, read comments here : https://aur.archlinux.org/packages/qgis-git

## Flatpak

There is an QGIS flatpak for QGIS Stable available, maintained by the flathub community.

For general Linux Flatpak install notes, see [the Flathub website](https://flatpak.org/setup).

[Here you can find QGIS on Flathub](https://flathub.org/apps/details/org.qgis.qgis).

To install:

```
flatpak install flathub org.qgis.qgis
```

The app should appear in your app launcher, alternatively use this command:

```
flatpak run org.qgis.qgis
```

To update your flatpak QGIS:

```
flatpak update
```

On certain distributions, you may also need to install xdg-desktop-portal or xdg-desktop-portal-gtk packages in order for file dialogs to appear.

Flathub files: https://github.com/flathub/org.qgis.qgis and report issues here: https://github.com/flathub/org.qgis.qgis/issues

### Extension Support
If you need to install additional Python modules, because they are needed by a plugin, you can install the module with (here installing the `scipy` module):

```
flatpak run --devel --command=pip3 org.qgis.qgis install scipy --user

# NOTE: it is possible you get an error like: "error: runtime/org.kde.Sdk/x86_64/5.15-23.08 not installed" then also do:
flatpak install runtime/org.kde.Sdk/x86_64/5.15-23.08
```

## Spack

Spack is a distro agnostic package manager for Linux, which is developed in the context of high-performance computing.

General info on installing Spack: https://github.com/spack/spack

QGIS package file on Spack: https://github.com/spack/spack/blob/develop/var/spack/repos/builtin/packages/qgis/package.py

To install:

```
spack install qgis
```

which builds and installs QGIS and **all** dependencies from scratch. Afterwards, QGIS can be used via:

```
spack load qgis
```

If additional python packages need to be installed, using a Spack environment is recommended. For example:

```
spack env create myenv
spack env activate -p myenv
spack add qgis py-lz4
spack install
```

Spack related issues should be reported at: https://github.com/spack/spack/issues

# Mac OS X / macOS


<!--
## QGIS nightly release

A nightly updated standalone installer from QGIS master can be downloaded from [here](/downloads/macos/qgis-macos-nightly.dmg).
-->


## Binary packages (installers)

Official All-in-one, signed installers for macOS High Sierra (10.13) and newer can be downloaded from the [QGIS download page]({{< ref "download" >}}).

After downloading QGIS, open the DMG file. Drag and drop the QGIS application into the Applications folder. The first launch attempt may fail due to Apple's security framework. 

**For macOS Sonoma and earlier:** To enable QGIS, command-click on its icon in your Applications folder and select ***Open*** in the context menu. A confirmation dialog will display where you need to click the ***Open*** button again. This only has to be done once.

**For macOS Sequoia and newer:** To enable QGIS, command-click its icon in your Applications folder and select ***Open*** from the context menu. A warning dialog will appear; click the ***Done*** button. Next, navigate to ***System Settings > Privacy & Security*** and scroll to the ***Security*** section. You should see a message stating that ***"QGIS" was blocked to protect your Mac***. Click ***Open Anyway***. A confirmation dialog will appear; click ***Open Anyway*** again. This only has to be done once.

## MacPorts

The package management system [MacPorts](https://www.macports.org) offers both the latest release version (port `qgis3`) and the long term version (port `qgis3-ltr`). This will install QGIS with native architecture, Intel x86_64 or Apple ARM. Main software dependencies such as GDAL, PDAL and GRASS GIS are usually the latest version available.

[Installing MacPorts and updating](https://guide.macports.org) it and the _ports_ are made with the _Terminal_. QGIS is however installed as an app bundle at `/Applications/MacPorts/QGIS3.app`.

Get information of a port:

```
sudo port info qgis3
```

Install port, e.g with GRASS GIS:

```
sudo port install qgis3 +grass
```

Update:

```
sudo port selfupdate
sudo port upgrade outdated
```

{{< rich-box-start icon="👩‍💻" layoutClass="tips">}}
{{< rich-content-start themeClass="coloring-1" >}}
##### Note

Concurrent installation of Homebrew and MacPorts is not compatible and will almost certainly lead to conflicts. If you choose to install one of the package systems you need to uninstall the other.
{{< rich-content-end >}}
{{< rich-box-end >}}

## Old releases

Previous releases of the official installer can be downloaded from https://qgis.org/downloads/macos/.

Previous releases of the kyngchaos installer can be downloaded from https://www.kyngchaos.com/software/archive/qgis/. The oldest installers support macOS 10.4 Tiger.

# FreeBSD

## QGIS stable

To install QGIS from binary packages type

```
pkg install qgis
```

Or to customize compilation options, you can build it from FreeBSD ports

```
cd /usr/ports/graphics/qgis
make install clean
```

## QGIS LTR

To install QGIS from binary packages type

```
pkg install qgis-ltr
```

Or to customize compilation options, you can build it from FreeBSD ports

```
cd /usr/ports/graphics/qgis-ltr
make install clean
```

# OpenBSD

QGIS Stable

To install QGIS from third-party package

```
pkg_add qgis
```

See:

- https://openbsd.app/?search=qgis for -stable OpenBSD
- https://openbsd.app/?search=qgis&current=on for -current OpenBSD

# QGIS Testing warning

{{< rich-box-start icon="⚠️" layoutClass="tips">}}
{{< rich-content-start themeClass="coloring-6" >}}
##### Warning

QGIS testing packages are provided for some platforms in addition to the QGIS stable version. QGIS testing contains unreleased software that is currently being worked on. They are only provided for testing purposes to early adopters to check if bugs have been resolved and that no new bugs have been introduced. Although we carefully try to avoid breakages, it may at any given time not work, or may do bad things to your data. Take care. You have been warned!
{{< rich-content-end >}}
{{< rich-box-end >}}

# Installing from Source

Please refer to [the developer installation notes](https://github.com/qgis/QGIS/blob/master/INSTALL.md) for notes on how to build and install QGIS from source for the different platforms we support.

{{< content-end >}}
