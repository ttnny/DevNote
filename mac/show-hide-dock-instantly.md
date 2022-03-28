# Show/Hide the macOS Dock Instantly

In **Terminal** app:

#### To enable:

On Intel-based Macs:

```
$ defaults write com.apple.dock autohide-delay -float 0 && killall Dock
```

On Apple Silicon-based Macs:

```
$ defaults write com.apple.dock autohide-delay -float 0 && defaults write com.apple.dock autohide-time-modifier -float 0.4 && killall Dock
```



#### To revert:

For the single setting on Intel-based Macs

```
$ defaults delete com.apple.dock autohide-delay && killall Dock
```

For the 2 commands on M1 Macs

```
$ defaults delete com.apple.dock autohide-delay && defaults delete com.apple.dock autohide-time-modifier && killall Dock
```



---

https://swissmacuser.ch/show-macos-dock-instantly-without-delay/