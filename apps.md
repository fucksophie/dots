# Automatic setup
Install automatically:
```
pacman -S ansible wget
wget https://raw.githubusercontent.com/yourfriendoss/dots/main/play.yml
ansible-galaxy collection install community.general
ansible-galaxy collection install kewlfft.aur
ansible-playbook -K play.yml
```

# Manual setup
[Visit here](manual.md), to install my setup manually (Warning, this might be out of date!).

# apps
Deps: `pkgconfig patch flex bison which`
1. Audio - `pipewire wireplumber pipewire-pulse pipewire-alsa pipewire-jack qpwgraph`
2. Wayland compositor - `hyprland xdg-desktop-portal-hyprland polkit-kde-agent`
3. Status bar - `waybar-hyprland-git`
4. Terminal - `alacritty`
5. Screenshotting - `wl-clipboard slurp grim`
6. Application picker - `rofi-lbonn-wayland`
7. Notifications - `mako`
8. Background - `swww`
9. File manager - `thunar gvfs gvfs-smb`
10. Dropbox - `dropbox thunar-dropbox dropbox-cli`
```
rm -rf ~/.dropbox-dist
install -dm0 ~/.dropbox-dist
```
11. Music - `spotify spicetify-cli`
```
curl -fsSL https://raw.githubusercontent.com/spicetify/spicetify-marketplace/main/resources/install.sh | sh
spicetify backup apply
```
12. Fonts - `ttf-jebrains-mono-nerd`
13. Discord - `vencord-desktop-git`
14. Browser - `vivaldi`
``` 
(credit: Archwiki)
Go to chrome://flags page, then search wayland. You will see the Preferred Ozone platform setting. Set it to auto. The default one is "X11". "Auto" selects Wayland if possible, X11 otherwise. 
```
15. Documents - `wps-office ttf-wps-fonts`
16. Code editor - `visual-studio-code-bin`
17. Extras - `pipes.sh neofetch`