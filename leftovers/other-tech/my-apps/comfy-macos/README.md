# 💻 comfy macOS

## MacOS Filesystem

### root

![](../../../../aaa-assets/comfy-macos-1.png)

* **Applications**: Contains all the applications installed on Mac
* **Library**: Stores fonts and other items used by applications
* **System**: macOS operating system
* **Users**: home folders of all users
* **Home**: holds your personal files and folders
* **Developer**: Appears only if Apple’s Developer Tools are installed, containing tools, documentation, and files for development
* **Network**: Contains network-related devices, servers, libraries, etc
* **Volumes**: Includes mounted devices and volumes, such as hard disks, CDs, DVDs, DMG mounts, etc
* **/bin**: Holds essential common binaries necessary for booting the operating system and its proper functioning
* **/etc**: Contains local system configuration files, including administrative and system files
* **/dev**: Includes device files representing peripheral devices like keyboards, mice, and trackpads
* **/usr**: containing subdirectories with information, configuration files, and other essentials used by the operating system
* **/sbin**: Contains essential system binaries and utilities for system, similar to Bin (/bin)
* **/tmp**: A directory for temporary files and caches
* **/var**: Holds variable data, which includes files whose contents change as the operating system runs

## Custom settings

#### links

 - [docs](https://macos-defaults.com/)

### Fast dock

- https://macos-defaults.com/dock/autohide-delay.html

```bash
# make fast
defaults write com.apple.dock "autohide-delay" -float "0" && killall Dock
# bring back
defaults delete com.apple.dock "autohide-delay" && killall Dock
```

### Active applications only

```sh
# enable
defaults write com.apple.dock "static-only" -bool "true" && killall Dock

# reset
defaults delete com.apple.dock "static-only" && killall Dock
```

