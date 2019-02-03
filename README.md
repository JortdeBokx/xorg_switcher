# xorg_switcher
Switches Xorg configurations depending on precense of egpu.
My Xorg configuration has been included as an example, it probably won't work for you unless you change the pci id's

### Installation:
* Clone this repository: `git clone https://github.com/JortdeBokx/xorg_switcher.git`
* First get your egpu's PCI_ID usign `lspci | grep 'VGA'`
* Next put your 'egpu' and 'laptop' configuration files somewhere
* Change the configuration files in `xorg_switch`:
  * Set `EGPU_PCI_ID` equal to your egpu's pci_id
  * Set `EGPU_CONFIG` equal to the location of the xorg config you'd like to use when your egpu is attached
  * Set `LAPTOP_CONFIG` equal to the location of the xorg config file you'd like to use when the egpu is not attached (i.e. teh 'default')
  * Set `PRODUCTION_CONFIG`equal to the location of the xorg config file that will be read by xorg.
* Move `xorg_switch` to `/usr/bin/xorg_switch` using `sudo cp xorg_switch /usr/bin/xorg_switch`
* Copy the startup service script to `/etc/systemd/system/xorg_switcher.service`: `sudo cp xorg_switcher.service /etc/systemd/system/xorg_switcher.service`
* Enable the script at startup using `sudo systemctl enable xorg_switcher.service`

### Usage:
The script should automatically select the desired xorg config file at startup. If you want to disable this feature simply set the `EGPU_PCI_ID` to `""` in the config section. 
If you want to switch xorg config files without restarting the system call `sudo xorg_switch laptop` or `sudo xorg_switch egpu`. Make sure you save all work before doing this as you'll need to restart your display manager.

### Contributions:
Have an idea or improvement? Let me know via [issues](https://github.com/JortdeBokx/xorg_switcher/issues) or create a pull request :)

## Tips for running Manjaro XFCE with nvidia graphics in an egpu
### Fix nvidia screen tearing on Manjaro with xfce4
An issue I personally encoutered was a lot of scren tearing on manjaro xfce, here is the fix I used:
* Add nvidia-drm.modeset=1 to GRUB_CMDLINE_LINUX_DEFAULT in `/etc/default/grub`. Then call `sudo update-grub`
* Add the following to `~/.config/autostart/screen_tearing_fix.desktop`:
  ``` Desktop
      [Desktop Entry]
      Encoding=UTF-8
      Name=Nvidia Screen Tearing Fix
      Exec=nvidia-settings --assign CurrentMetaMode="nvidia-auto-select +0+0 { ForceCompositionPipeline = On }"
      Terminal=false
      Type=Application
    ```
### Fix no sound via nvidia graphics card
If you get no sound via the monitors connected to the grahpics card, disable displayport 1.2 on the monitor.
