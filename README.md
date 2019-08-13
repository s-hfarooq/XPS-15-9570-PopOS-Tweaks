# XPS-15-9570-Ubuntu-Tweaks

## Install Pop!_OS
  Create a Pop!_OS installation media by downloading the ISO from [System76's website](https://system76.com/pop) (NVIDIA variant since this laptop has a 1050ti maxq). Once downloaded, plug in a USB stick and use a tool like [Rufus](https://rufus.ie/) or [Etcher](https://www.balena.io/etcher/) to create the installation media.

  Restart the system and press F12 on boot to get into the boot menu. Boot in UEFI mode from the installer you just created. From there, follow on screen instructions to install Pop!_OS.

## Update and Fix Boot
  Once the OS is installed, update the system:

      sudo apt-get update
      sudo apt-get upgrade
      sudo apt-get dist-upgrade


  Install Boot Repair so that grub is properly installed and you can boot into Windows.

    sudo add-apt-repository ppa:yannubuntu/boot-repair
    sudo apt-get update
    sudo apt-get install -y boot-repair && boot-repair)

## Fix Sleep Mode
  By default, the XPS uses `s2idle` sleep mode which drains battery pretty quickly. `deep` sleep reduces the power draw in sleep mode without any noticeable drawbacks. Run:

    sudo bash -c 'echo deep > /sys/power/mem_sleep'

  to see if the problem is fixed. If so, run the following line:

    sudo -H gedit /etc/default/grub.

  and replace `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"` with `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash mem_sleep_default=deep"`

  Finally, regenerate your grub configuration with:

    sudo grub-mkconfig -o /boot/grub/grub.cfg

## Install packages
install tlp, powertop, gnome-tweak-tool, gnome-shell-extensions, vim, g++, xfce4-terminal, exfat-fuse, exfat-utils

edit tlp settings
  sudo nano /etc/default/tlp

  CPU_SCALING_GOVERNOR_ON_BAT=powersave
  DEVICES_TO_DISABLE_ON_STARTUP="bluetooth"

  disable bluetooth on startup

gnome additions
  user themes (extensions.gnome.org/extension/19/user-themes)
  dash to dock (extensions.gnome.org/extension/307/dash-to-dock)
  CPU power manager (extensions.gnome.org/extension/945/cpu-power-manager)

settings
  change background
  power -> when power button pressed suspend, turn off bluetooth to save power
  devices -> keyboard add crtl+alt+t open xfce4-terminal
              mouse/touchpad natural scrolling

gnome tweaks
  apperance -> applications to pop-slim, shell to pop-dark-slim
  font -> interface to SFM
    github.com/supermarin/YosemiteSanFranciscoFont - SFNS regular
  startup applications -> discord, firefox
  top bar -> battery percentage
  windows ->titlebar buttons -> minimize, placement left

install programs (discord, atom, chrome, xfce4-terminal, lollypop, vlc)
  lollypop -> sudo apt-add-repository ppa:gnumdk/lollypop
              sudo apt-get update
              sudo apt-get install lollypop
    atom themes
        atom-material-ui (atom material as UI theme, atom dark as syntax theme)
        platformio-ide-terminal
        autocomplete-clang
        autocomplete-html-entities
        autocomplete-java
        autocomplete-python
