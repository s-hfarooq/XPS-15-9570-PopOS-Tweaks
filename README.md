# XPS-15-9570-PopOS-Tweaks

## Install Pop!_OS
  Create a Pop!_OS installation media by downloading the ISO from [System76's website](https://system76.com/pop) (NVIDIA variant since this laptop has a 1050Ti Max-Q). Use the 19.10 version. Once downloaded, plug in a USB stick and use a tool like [Rufus](https://rufus.ie/) or [Etcher](https://www.balena.io/etcher/) to create the installation media.

  Restart the system and press F12 on boot to get into the boot menu. Boot in UEFI mode from the installer you just created. From there, follow on screen instructions to install Pop!_OS.

## Update and Fix Boot
  Once the OS is installed, update the system:

      sudo apt update
      sudo apt upgrade
      sudo apt dist-upgrade


  Install Boot Repair so that grub is properly installed and you can boot into Windows.

    sudo add-apt-repository ppa:yannubuntu/boot-repair
    sudo apt update
    sudo apt install -y boot-repair && boot-repair

## Fix Sleep Mode
  By default, the XPS 15 uses `s2idle` sleep mode which drains battery pretty quickly. `deep` sleep reduces the power draw in sleep mode without any noticeable drawbacks. Run:

    sudo bash -c 'echo deep > /sys/power/mem_sleep'

  to see if you can swich over to `deep` sleep to fix the problem. If everything works, run the following line:

    sudo -H gedit /etc/default/grub

  and replace `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"` with `GRUB_CMDLINE_LINUX_DEFAULT="quiet splash mem_sleep_default=deep"` to make the switch permanent.

  Finally, regenerate your grub configuration with:

    sudo grub-mkconfig -o /boot/grub/grub.cfg

## Fix Intel Screen Tearing
  NOTE: This doesn't seem to be necessary with 19.10, but I'll keep it here just in case.
  
  Fix the screen tearing that occurs with integrated graphics

    sudo mkdir /etc/X11/xorg.conf.d
    sudo gedit /etc/X11/xorg.conf.d/20-intel-graphics.conf

  Within that file place the following

    Section "Device"
      Identifier  "Intel Graphics"
      Driver      "intel"
      Option      "TripleBuffer" "true"
      Option      "TearFree"     "true"
      Option      "DRI"          "false"
    EndSection

  Then reboot

## Fix Dual Boot Time Issue
  When dual booting Ubuntu and Windows, an issue occurs where the time in Windows is incorrect after rebooting from Ubuntu. To fix this , make Ubuntu use local time

    timedatectl set-local-rtc 1 --adjust-system-clock

## Install Packages

  Install tlp, powertop, gnome-tweak-tool, gnome-shell-extensions, vim, g++, xfce4-terminal, exfat-fuse, and exfat-utils. Do this by running

    sudo apt install tlp powertop gnome-tweak-tool gnome-shell-extensions vim g++ xfce4-terminal exfat-fuse exfat-utils

  Install [`comfortable-swipe`](https://github.com/Hikari9/comfortable-swipe) to enable trackpad gestures. I personally never really used it, instead opting for keyboard shortcuts.

    sudo apt install git libinput-tools libxdo-dev g++
    git clone https://github.com/Hikari9/comfortable-swipe.git --depth 1
    cd comfortable-swipe
    bash install
    sudo gpasswd -a $USER $(ls -l /dev/input/event* | awk '{print $4}' | head --line=1)

  Restart then run

    comfortable-swipe start

  Configure settings for comfortable-swipe

    gedit $(comfortable-swipe config)
    comfortable-swipe restart

## Edit TLP Settings
  Edit the TLP settings to further reduce power drain. Run

    sudo gedit /etc/default/tlp

  to open up the file. Uncomment out the following lines in the file and make sure the things on the right of the equal sign are the same in your configuration. Alternatively, replace your TLP configuration file with `tlp_new`. These two lines ensure that the CPU swiches to powersave mode when the laptop is unplugged and makes sure that Bluetooth isn't automatically turned on when the system is restarted.

    CPU_SCALING_GOVERNOR_ON_BAT=powersave
    DEVICES_TO_DISABLE_ON_STARTUP="bluetooth"

## Additional Power Savings
  Run the following to disable the fingerprint sensor since it doesn't work in Linux

    sudo bash -c 'echo "# Disable fingerprint reader SUBSYSTEM==\"usb\", ATTRS{idVendor}==\"27c6\", ATTRS{idProduct}==\"5395\", ATTR{authorized}=\"0\"" > /etc/udev/rules.d/fingerprint.rules'

## GNOME Additions
  Download the following additions and enable them

  * [User Themes](https://extensions.gnome.org/extension/19/user-themes)
  * [Dash to Dock](https://extensions.gnome.org/extension/307/dash-to-dock)
  * [CPU Power Manager](https://extensions.gnome.org/extension/945/cpu-power-manager)
  * [Activities Configurator](https://extensions.gnome.org/extension/358/activities-configurator/)

## Settings
  Changes to make in settings:
  * Change background and lock screen images
  * In power, change:
    * `When the Power Button is pressed` to `Suspend`
    * `Turn off Bluetooth to save power` to `On`
  * In devices, change
    * Keyboard -> add `crtl+alt+t` to open `xfce4-terminal`
    * Mouse & Touchpad -> `Natural Scrolling` to `On`

## GNOME Tweaks
  Changes to make in GNOME Tweaks
  * Appearance
    * Themes -> `Applications` to `Pop-slim`
    * Themes -> shell to `Pop-dark-slim`
  * Fonts
    * `Interface` to `SFNS Display Regular 10`
      * Downloaded from [here](https://github.com/supermarin/YosemiteSanFranciscoFont)
  * Startup Applications
    * Add `Discord` and `Firefox`
  * Top Bar
    * `Battery Percentage` to `On`
  * Windows
    * Tilebar Buttons -> `Minimize` to `On`
    * Tilebar Buttons -> `Placement` to `left`

## Install Additional Programs
  Make sure to install:
  * Discord
  * Atom
  * Chrome
  * xfce4-terminal
  * Lollypop
  * VLC


  To install Lollypop:

    sudo apt-add-repository ppa:gnumdk/lollypop
    sudo apt update
    sudo apt install lollypop

  Install these add-ons in Atom
  * atom-material-ui
    * `atom material` as UI theme, `atom dark` as syntax theme
  * platformio-ide-terminal
  * autocomplete-clang
  * autocomplete-html-entities
  * autocomplete-java
  * autocomplete-python
