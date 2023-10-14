# initial commands (will be done by ansible)

Make new user
```
pacman -S sudo
useradd -mU yf
echo "yf ALL=(ALL:ALL) ALL" >> /etc/sudoers
passwd yf
su yf
```

Install yay
```
sudo pacman -S git fakeroot make gcc
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
cd ..
rm -rf yay
```

Setup getty autologin
```
mkdir /etc/systemd/system/getty@tty1.service.d

cat <<- "EOF" > /etc/systemd/system/getty@tty1.service.d/skip-username.conf
[Service]
ExecStart=
ExecStart=-/sbin/agetty -o '-p -- yf' --noclear --skip-login - $TERM
Environment=XDG_SESSION_TYPE=wayland
EOF
``` 

Remove password
```
sudo sed -i 's/yf:[^:]*/yf:/' /etc/shadow
```

Install stow
```
yay -S stow
```

Install dotfiles
```
git clone --recurse-submodules https://github.com/yourfriendoss/dots .shell
cd .shell
stow src
```

Automatically log into Hyprland
```
cat <<- "EOF" > /home/yf/.bash_profile
if [[ "$(tty)" == "/dev/tty1" ]]
then
Hyprland
fi
EOF
```

# apps
Deps: `pkgconfig patch flex bison which`
1. Audio - `pipewire wireplumber pipewire-pulse pipewire-alsa pipewire-jack qpwgraph` (dots are not needed)
2. Wayland compositor - `hyprland xdg-desktop-portal-hyprland polkit-kde-agent` (.config/hypr)
3. Status bar - `waybar-hyprland-git` (.config/waybar)
4. Terminal - `alacritty` (dots are not needed)
5. Screenshotting - `wl-clipboard slurp grim` (dots are not needed)
6. Application picker - `rofi-lbonn-wayland` (.config/rofi)
7. Notifications - `mako`  (.config/mako)
8. Background - `swww` (wallpaper.jpg)
**From now on, you can start Hyprland**
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
13. Discord - `discord`
```
sudo chown -R $USER:$USER /opt/discord
sh -c "$(curl -sS https://raw.githubusercontent.com/Vendicated/VencordInstaller/main/install.sh)"
```
Install vencord, and then install openasar!
14. Browser - `vivaldi`
``` 
(credit: Archwiki)
Go to chrome://flags page, then search wayland. You will see the Preferred Ozone platform setting. Set it to auto. The default one is "X11". "Auto" selects Wayland if possible, X11 otherwise. 
```
