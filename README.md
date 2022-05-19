#!/bin/bash

sudo tee /etc/sudoers.d/$USER <<< "$USER ALL=(ALL) NOPASSWD: ALL"
sudo chmod 440 /etc/sudoers.d/$USER
sed -i '30,33d' $HOME/.config/powermanagementprofilesrc
sed -i '18,25d' $HOME/.config/powermanagementprofilesrc
sed -i '3,9d' $HOME/.config/powermanagementprofilesrc
echo -e "[Daemon]\nAutolock=false\nLockOnResume=false" | tee $HOME/.config/kscreenlockerrc
echo "alias mountsdb=\"mkdir $HOME/sdb; sudo mount /dev/sdb1 $HOME/sdb\"" | tee -a $HOME/.bashrc $HOME/.zshrc
echo "alias umountsdb=\"sudo umount -v /dev/sdb1\"" | tee -a $HOME/.bashrc $HOME/.zshrc
sudo pacman -R --noconfirm matray
sudo pacman-mirrors --geoip -m rank
sudo pacman -Syyu --noconfirm
sudo pacman -Fy --noconfirm
sudo pacman -S --noconfirm archlinux-keyring

timeout 9 firefox
ffUserRelease=$(find /home/$USER/.mozilla/firefox/ -type d -name '*default-release')
echo -e "\
//user.js
user_pref(\"network.proxy.http\", \"127.0.0.1\");\n\
user_pref(\"network.proxy.http_port\", 7890);\n\
user_pref(\"network.proxy.ssl\", \"127.0.0.1\");\n\
user_pref(\"network.proxy.ssl_port\", 7890);\n\
user_pref(\"network.proxy.socks\", \"127.0.0.1\");\n\
user_pref(\"network.proxy.socks_port\", 7890);\n\
user_pref(\"network.proxy.socks_remote_dns\", true);\n\
user_pref(\"network.trr.mode\", 3);\n\
user_pref(\"network.trr.uri\", \"https://mozilla.cloudflare-dns.com/dns-query\");\n\
user_pref(\"network.trr.custom_uri\", \"https://a.passcloud.xyz/dns-query\");\n\
user_pref(\"network.dns.echconfig.enabled\", true);\n\
user_pref(\"network.dns.http3_echconfig.enabled\", true);\n\
user_pref(\"network.dns.use_https_rr_as_altsvc\", true);\n\
user_pref(\"dom.security.https_only_mode\", true);\n\
user_pref(\"media.ffmpeg.vaapi.enabled\", true);\n\
user_pref(\"media.ffvpx.enabled\", false);\n\
user_pref(\"media.rdd-vpx.enabled\", false);\n\
user_pref(\"media.av1.enabled\", false);\n\
" | tee $ffUserRelease/user.js

sudo pacman -S --noconfirm qt5-3d qt5-base qt5-charts qt5-connectivity qt5-datavis3d qt5-declarative qt5-doc qt5-examples qt5-gamepad qt5-graphicaleffects qt5-imageformats qt5-location qt5-lottie qt5-multimedia qt5-networkauth qt5-purchasing qt5-quick3d qt5-quickcontrols qt5-quickcontrols2 qt5-quicktimeline qt5-remoteobjects qt5-script qt5-scxml qt5-sensors qt5-serialbus qt5-serialport qt5-speech qt5-svg qt5-tools qt5-translations qt5-virtualkeyboard qt5-wayland qt5-webchannel qt5-webengine qt5-webglplugin qt5-websockets qt5-webview qt5-x11extras qt5-xmlpatterns
sudo pacman -S --noconfirm texstudio texlive-core texlive-bibtexextra texlive-fontsextra texlive-formatsextra texlive-latexextra texlive-pictures texlive-pstricks texlive-publishers texlive-science
sudo pacman -S --noconfirm libxau libxi libxss libxtst libxcursor libxcomposite libxdamage libxfixes libxrandr libxrender mesa-libgl alsa-lib libglvnd
sudo pacman -S --noconfirm fcitx5 fcitx5-configtool fcitx5-gtk fcitx5-qt fcitx5-chinese-addons
sudo pacman -S --noconfirm gvfs-smb samba kdenetwork-filesharing manjaro-settings-samba
sudo pacman -S --noconfirm base-devel ninja openmpi tbb cmake make python python-numpy
sudo pacman -S --noconfirm wine winetricks wine-mono wine_gecko
sudo pacman -S --noconfirm intel-gpu-tools
sudo pacman -S --noconfirm android-tools
sudo pacman -S --noconfirm net-tools
sudo pacman -S --noconfirm glfw-x11
sudo pacman -S --noconfirm nmap
sudo pacman -S --noconfirm barrier
sudo pacman -S --noconfirm git
sudo pacman -S --noconfirm wget
sudo pacman -S --noconfirm zip
sudo pacman -S --noconfirm unzip
sudo pacman -S --noconfirm vlc
sudo pacman -S --noconfirm libreoffice
sudo pacman -S --noconfirm octave
sudo pacman -S --noconfirm freerdp
sudo pacman -S --noconfirm remmina
sudo pacman -S --noconfirm kdenlive
sudo pacman -S --noconfirm gimp
sudo pacman -S --noconfirm inkscape
sudo pacman -S --noconfirm audacity
sudo pacman -S --noconfirm obs-studio
sudo pacman -S --noconfirm flameshot
sudo pacman -S --noconfirm rnote
sudo pacman -S --noconfirm atom
sudo pacman -S --noconfirm apm
sudo pacman -S --noconfirm baidupcs-go
sudo pacman -S --noconfirm wireguard-tools
sudo pacman -S --noconfirm paraview
sudo pacman -S --noconfirm yt-dlp
sudo pacman -S --noconfirm chromium && xdg-settings set default-web-browser chromium.desktop
sudo pacman -S --noconfirm syncthing && sudo systemctl enable --now syncthing@$USER
sudo pacman -S --noconfirm clash && clash -t
echo "alias exportproxy=\"clash > /dev/null 2>&1 & ; export http_proxy=http://127.0.0.1:7890/ ; export https_proxy=https://127.0.0.1:7890/ ; export ftp_proxy=http://127.0.0.1:7890/\"" | tee -a $HOME/.bashrc $HOME/.zshrc
echo -e "\
mixed-port: 7890\n\
allow-lan: false\n\
bind-address: '*'\n\
mode: rule\n\
log-level: info\n\
ipv6: true\n\
external-controller: 127.0.0.1:9090\n\
secret: '0000'\n\
rules:\n\
  - DOMAIN-SUFFIX,cn,DIRECT\n\
  - GEOIP,CN,DIRECT\n\
  - MATCH,tDE\n\
proxies:\n\
  - {name: \"tDE\", type: trojan, port: 443, server: c.c, password: 0}\n\
  - {name: \"tSG\", type: trojan, port: 443, server: c.c, password: 0}\n\
  - {name: \"sDE\", type: ss, port: 443, server: a.a.a.a, password: 0, cipher: chacha20-ietf-poly1305}\n\
  - {name: \"sSG\", type: ss, port: 443, server: a.a.a.a, password: 0, cipher: chacha20-ietf-poly1305}\n\
" | tee $HOME/.config/clash/config.yaml

sudo pacman -S --noconfirm virt-manager qemu vde2 dnsmasq bridge-utils openbsd-netcat edk2-ovmf swtpm
yes | sudo pacman -S iptables-nft
sudo systemctl daemon-reload
sudo systemctl enable libvirtd
sudo usermod -aG kvm $USER
sudo usermod -aG libvirt $USER
sudo systemctl start libvirtd

sudo pacman -S --noconfirm docker
sudo systemctl daemon-reload
sudo systemctl enable docker
sudo usermod -aG docker $USER
sudo systemctl start docker
curl -s https://api.github.com/repos/docker/compose/releases/latest | grep browser_download_url  | grep docker-compose-linux-x86_64 | cut -d '"' -f 4 | wget -qi -
chmod +x docker-compose-linux-x86_64
sudo mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose
# echo -e '{\n "registry-mirrors": ["https://registry.docker-cn.com"] \n}' | sudo tee /etc/docker/daemon.json
# curl -s https://api.github.com/repos/gorhill/uBlock/releases/latest | grep "uBlock*" | cut -d : -f 2,3 | tr -d \" | wget -qi - ; rm -rf $HOME/Downloads/*,

source /etc/profile
source /etc/environment
source $HOME/.bash_profile
source $HOME/.bashrc
source $HOME/.zshrc
yes | sudo pacman -Scc

wget -O - https://www.anaconda.com/distribution/ 2>/dev/null | sed -ne 's@.*\(https:\/\/repo\.anaconda\.com\/archive\/Anaconda3-.*-Linux-x86_64\.sh\)\">64-Bit (x86) Installer.*@\1@p' | xargs wget
bash ./Anaconda3-*-Linux-x86_64.sh -b -p $HOME/anaconda3
rm -rf ./Anaconda3-*-Linux-x86_64.sh
echo -e "\
[Desktop Entry]\n\
Type=Application\n\
Name=Spyder\n\
Exec=$HOME/anaconda3/bin/spyder\n\
Terminal=false\n\
StartupNotify=true\n\
Categories=Science\n\
" | sudo tee /usr/share/applications/spyder.desktop

wget -P $HOME/Downloads https://dl.openfoam.com/source/v2112/OpenFOAM-v2112.tgz
wget -P $HOME/Downloads https://dl.openfoam.com/source/v2112/ThirdParty-v2112.tgz
mkdir $HOME/openfoam
tar -zxvf $HOME/Downloads/OpenFOAM-v2112.tgz -C $HOME/openfoam/
tar -zxvf $HOME/Downloads/ThirdParty-v2112.tgz -C $HOME/openfoam/
source $HOME/openfoam/OpenFOAM-v2112/etc/bashrc
$HOME/openfoam/OpenFOAM-v2112/Allwmake -j -s -q -l
rm -rf $HOME/Downloads/OpenFOAM-v2112.tgz $HOME/Downloads/ThirdParty-v2112.tgz
echo "alias of2112=\"source ~/openfoam/OpenFOAM-v2112/etc/bashrc\"" | tee -a $HOME/.bashrc $HOME/.zshrc


<< EOF
exportproxy && for iAPK in {\
"org.mozilla.fenix",\
"com.google.android.inputmethod.latin",\
"com.google.android.apps.translate",\
"com.adobe.reader",\
"org.videolan.vlc",\
"com.jacksoftw.webcam",\
"com.tencent.mm",\
"com.bilibili.app.in",\
"com.xiaomi.smarthome",\
"com.whatsapp",\
"com.spotify.music",\
"com.microsoft.teams",\
"com.tranzmate"\
"it.kyntos.webus"\
}; do docker run --rm -v $HOME/Downloads:/output ghcr.io/efforg/apkeep:stable -a $iAPK /output && adb install "$iAPK.apk"; done

sudo su;\
apt update -y;\
apt install iptables-persistent wget curl -y;\
iptables -I INPUT -s 0.0.0.0/0 -p tcp --match multiport --dports 22,80,443 -j ACCEPT;\
iptables-save;\
netfilter-persistent save;\
netfilter-persistent reload;\
iptables -L -n --line-numbers
EOF
