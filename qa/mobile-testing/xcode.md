# ☄️ Xcode

## Simulator.app Shortcut

open

```
/Applications/Xcode.app/Contents/Developer/Applications/Simulator.app
```

## Safari integration

Xcode + Safari devtools integration

* open Safari in Simulator
* find Simulator device in develop section in Safari menu

## commands

- [link](https://macos-defaults.com/simulator/screenshotsavelocation.html)

```bash
# set
defaults write com.apple.iphonesimulator "ScreenShotSaveLocation" -string "~/Pictures/Simulator Screenshots"

# reset
defaults delete com.apple.iphonesimulator "ScreenShotSaveLocation"
```