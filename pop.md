install pop on usb using rufus or etcher

boot onto usb using uefi mode

follow on screen instructions to install pop

update (update, upgrade, dist-upgrade)

install boot repair to fix boot (sudo add-apt-repository ppa:yannubuntu/boot-repair
sudo apt-get update
sudo apt-get install -y boot-repair && boot-repair)

fix sleep
  I had high drain when in sleep mode, and it turned out the laptop wasn't entering proper deep sleep. Run:

  cat /sys/power/mem_sleep - you should see s2idle and deep, with one surrounded by square brackets to show it's activated. I suspect you have s2idle highlighted (the poor sleep state). To fix, sudo bash -c 'echo deep > /sys/power/mem_sleep' (as root).

  If that fixes it, add the following to your kernel cmdline in your bootloader:

  mem_sleep_default=deep



    sudo -H gedit /etc/default/grub. Replace the line

  GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"

  with

  GRUB_CMDLINE_LINUX_DEFAULT="quiet splash mem_sleep_default=deep"

  and regenerate your grub configuration (run sudo grub-mkconfig -o /boot/grub/grub.cfg)


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
