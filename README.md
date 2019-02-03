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
