# Manually install my setup
## I reccomend to use the automatic setup outlined in apps.md instead.
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