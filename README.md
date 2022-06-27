#!/bin/bash

sudo tee /etc/sudoers.d/$USER <<< "$USER ALL=(ALL) NOPASSWD: ALL"
sudo chmod 440 /etc/sudoers.d/$USER
sed -i '30,33d' $HOME/.config/powermanagementprofilesrc
sed -i '18,25d' $HOME/.config/powermanagementprofilesrc
sed -i '3,9d' $HOME/.config/powermanagementprofilesrc
echo -e "[Daemon]\nAutolock=false\nLockOnResume=false" | tee $HOME/.config/kscreenlockerrc
sudo pacman -R --noconfirm matray
sudo pacman-mirrors --geoip -m rank
sudo pacman -Syyu --noconfirm
sudo pacman -Fy --noconfirm
sudo pacman -S --noconfirm archlinux-keyring
balooctl suspend
balooctl disable
echo "alias mountsdb=\"mkdir $HOME/sdb; sudo mount /dev/sdb1 $HOME/sdb; sudo chmod -R 777 $HOME/sdb\"" | tee -a $HOME/.bashrc $HOME/.zshrc
echo "alias umountsdb=\"sudo umount -v /dev/sdb1\"" | tee -a $HOME/.bashrc $HOME/.zshrc
sudo sed -i 's/RefreshPeriod = ./RefreshPeriod = 0/g' /etc/pamac.conf

timeout 9 firefox
sudo sed -i 's|Exec=|Exec=export MOZ_DISABLE_RDD_SANDBOX=1 \&\& |g' /usr/share/applications/firefox.desktop
ffUserRelease=$(find /home/$USER/.mozilla/firefox/ -type d -name '*default-release')
echo -e "\
user_pref(\"network.trr.mode\", 3);\n\
user_pref(\"network.trr.uri\", \"https://mozilla.cloudflare-dns.com/dns-query\");\n\
user_pref(\"network.trr.custom_uri\", \"https://a.passcloud.xyz/dns-query\");\n\
user_pref(\"network.dns.echconfig.enabled\", true);\n\
user_pref(\"network.dns.use_https_rr_as_altsvc\", true);\n\
user_pref(\"dom.security.https_only_mode\", true);\n\
user_pref(\"network.proxy.type\", 5);\n\
user_pref(\"network.proxy.http\", \"127.0.0.1\");\n\
user_pref(\"network.proxy.http_port\", 7890);\n\
user_pref(\"network.proxy.ssl\", \"127.0.0.1\");\n\
user_pref(\"network.proxy.ssl_port\", 7890);\n\
user_pref(\"network.proxy.socks\", \"127.0.0.1\");\n\
user_pref(\"network.proxy.socks_port\", 7890);\n\
user_pref(\"network.proxy.socks_remote_dns\", true);\n\
user_pref(\"media.ffmpeg.vaapi.enabled\", true);\n\
user_pref(\"media.ffvpx.enabled\", false);\n\
user_pref(\"media.rdd-vpx.enabled\", false);\n\
user_pref(\"media.av1.enabled\", false);\n\
user_pref(\"browser.startup.homepage\", \"https://translate.google.com/\");\n\
" | tee $ffUserRelease/user.js

sudo pacman -S --noconfirm qt5-3d qt5-base qt5-charts qt5-connectivity qt5-datavis3d qt5-declarative qt5-doc qt5-examples qt5-gamepad qt5-graphicaleffects qt5-imageformats qt5-location qt5-lottie qt5-multimedia qt5-networkauth qt5-purchasing qt5-quick3d qt5-quickcontrols qt5-quickcontrols2 qt5-quicktimeline qt5-remoteobjects qt5-script qt5-scxml qt5-sensors qt5-serialbus qt5-serialport qt5-speech qt5-svg qt5-tools qt5-translations qt5-virtualkeyboard qt5-wayland qt5-webchannel qt5-webengine qt5-webglplugin qt5-websockets qt5-webview qt5-x11extras qt5-xmlpatterns
sudo pacman -S --noconfirm texstudio texlive-core texlive-bibtexextra texlive-fontsextra texlive-formatsextra texlive-latexextra texlive-pictures texlive-pstricks texlive-music texlive-publishers texlive-science texlive-humanities texlive-games texlive-langchinese texlive-langjapanese texlive-langkorean texlive-langextra
sudo pacman -S --noconfirm libxau libxi libxss libxtst libxcursor libxcomposite libxdamage libxfixes libxrandr libxrender mesa-libgl alsa-lib libglvnd
sudo pacman -S --noconfirm ruby perl-image-exiftool gitlab-workhorse gitlab-gitaly redis gitlab-shell gitlab yarn
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
sudo pacman -S --noconfirm octave
sudo pacman -S --noconfirm freerdp
sudo pacman -S --noconfirm remmina
sudo pacman -S --noconfirm libreoffice
sudo pacman -S --noconfirm kdenlive
sudo pacman -S --noconfirm gimp
sudo pacman -S --noconfirm inkscape
sudo pacman -S --noconfirm audacity
sudo pacman -S --noconfirm obs-studio
sudo pacman -S --noconfirm flameshot
sudo pacman -S --noconfirm rnote
sudo pacman -S --noconfirm wireguard-tools
sudo pacman -S --noconfirm paraview
sudo pacman -S --noconfirm yt-dlp
sudo pacman -S --noconfirm spyder && pip install pystun3 numpy pandas matplotlib scienceplots
sudo pacman -S --noconfirm chromium && xdg-settings set default-web-browser chromium.desktop
sudo pacman -S --noconfirm syncthing && sudo systemctl enable --now syncthing@$USER
sudo pacman -S --noconfirm clash && clash -t
echo "alias exportproxy=\"export http_proxy=http://127.0.0.1:7890/ ; export https_proxy=https://127.0.0.1:7890/ ; export ftp_proxy=http://127.0.0.1:7890/\"" | tee -a $HOME/.bashrc $HOME/.zshrc
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
  - GEOIP,CN,DIRECT\n\
  - DOMAIN-SUFFIX,cn,DIRECT\n\
  - DOMAIN-SUFFIX,nature.com,DIRECT\n\
  - DOMAIN-SUFFIX,springer.com,DIRECT\n\
  - DOMAIN-SUFFIX,springernature.com,DIRECT\n\
  - DOMAIN-SUFFIX,cdnsciencepub.com,DIRECT\n\
  - DOMAIN-SUFFIX,academic.oup.com,DIRECT\n\
  - DOMAIN-SUFFIX,sciencedirect.com,DIRECT\n\
  - DOMAIN-SUFFIX,plos.org,DIRECT\n\
  - DOMAIN-SUFFIX,frontiersin.org,DIRECT\n\
  - DOMAIN-SUFFIX,arxiv.org,DIRECT\n\
  - DOMAIN-SUFFIX,ieee.org,DIRECT\n\
  - DOMAIN-SUFFIX,mdpi.com,DIRECT\n\
  - DOMAIN-SUFFIX,biomedcentral.com,DIRECT\n\
  - DOMAIN-SUFFIX,elsevier.com,DIRECT\n\
  - DOMAIN-SUFFIX,ascelibrary.org,DIRECT\n\
  - DOMAIN-SUFFIX,taylorandfrancis.com,DIRECT\n\
  - DOMAIN-SUFFIX,cshlp.org,DIRECT\n\
  - DOMAIN-SUFFIX,wiley.com,DIRECT\n\
  - DOMAIN-SUFFIX,acs.org,DIRECT\n\
  - DOMAIN-SUFFIX,cnki.net,DIRECT\n\
  - DOMAIN-SUFFIX,tandfonline.com,DIRECT\n\
  - DOMAIN-SUFFIX,csiro.au,DIRECT\n\
  - DOMAIN-SUFFIX,clarivate.com,DIRECT\n\
  - DOMAIN-SUFFIX,annualreviews.org,DIRECT\n\
  - DOMAIN-SUFFIX,jstor.org,DIRECT\n\
  - DOMAIN-SUFFIX,rsc.org,DIRECT\n\
  - DOMAIN-SUFFIX,clarivate.com,DIRECT\n\
  - DOMAIN-SUFFIX,webofscience.com,DIRECT\n\
  - DOMAIN-SUFFIX,webofknowledge.com,DIRECT\n\
  - DOMAIN-SUFFIX,liebertpub.com,DIRECT\n\
  - DOMAIN-SUFFIX,science.org,DIRECT\n\
  - DOMAIN-SUFFIX,worldscientific.com,DIRECT\n\
  - DOMAIN-SUFFIX,netflix.com,tSG\n\
  - DOMAIN-SUFFIX,netflix.net,tSG\n\
  - DOMAIN-SUFFIX,nflxext.com,tSG\n\
  - DOMAIN-SUFFIX,nflximg.com,tSG\n\
  - DOMAIN-SUFFIX,nflximg.net,tSG\n\
  - DOMAIN-SUFFIX,nflxso.net,tSG\n\
  - DOMAIN-SUFFIX,nflxvideo.net,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest0.com,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest1.com,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest2.com,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest3.com,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest4.com,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest5.com,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest6.com,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest7.com,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest8.com,tSG\n\
  - DOMAIN-SUFFIX,netflixdnstest9.com,tSG\n\
  - IP-CIDR,23.246.0.0/18,tSG,no-resolve\n\
  - IP-CIDR,37.77.184.0/21,tSG,no-resolve\n\
  - IP-CIDR,45.57.0.0/17,tSG,no-resolve\n\
  - IP-CIDR,64.120.128.0/17,tSG,no-resolve\n\
  - IP-CIDR,66.197.128.0/17,tSG,no-resolve\n\
  - IP-CIDR,108.175.32.0/20,tSG,no-resolve\n\
  - IP-CIDR,192.173.64.0/18,tSG,no-resolve\n\
  - IP-CIDR,198.38.96.0/19,tSG,no-resolve\n\
  - IP-CIDR,198.45.48.0/20,tSG,no-resolve\n\
  - MATCH,tDE\n\
proxies:\n\
  - {name: \"tDE\", type: trojan, port: 443, server: c.c, password: 0}\n\
  - {name: \"tSG\", type: trojan, port: 443, server: c.c, password: 0}\n\
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

source /etc/profile
source /etc/environment
source $HOME/.bash_profile
source $HOME/.bashrc
source $HOME/.zshrc
yes | sudo pacman -Scc

sudo sed -i 's/#EnableAUR/EnableAUR/g' /etc/pamac.conf
sudo pamac install --no-confirm wps-office ttf-wps-fonts wps-office-mui-zh-cn wps-office-fonts ttf-ms-fonts wps-office-all-dicts-win-languages
sudo sed -i '2a \
export XMODIFIERS="@im=fcitx"\
export QT_IM_MODULE="fcitx"' /usr/bin/wps /usr/bin/et /usr/bin/wpp /usr/bin/wpspdf
sudo sed -i 's/EnableAUR/#EnableAUR/g' /etc/pamac.conf

wget -P $HOME/Downloads https://dl.openfoam.com/source/v2112/OpenFOAM-v2112.tgz
wget -P $HOME/Downloads https://dl.openfoam.com/source/v2112/ThirdParty-v2112.tgz
mkdir $HOME/openfoam
tar -zxvf $HOME/Downloads/OpenFOAM-v2112.tgz -C $HOME/openfoam/
tar -zxvf $HOME/Downloads/ThirdParty-v2112.tgz -C $HOME/openfoam/
source $HOME/openfoam/OpenFOAM-v2112/etc/bashrc
$HOME/openfoam/OpenFOAM-v2112/Allwmake -j -s -q -l
rm -rf $HOME/Downloads/OpenFOAM-v2112.tgz $HOME/Downloads/ThirdParty-v2112.tgz
echo "alias of2112=\"source ~/openfoam/OpenFOAM-v2112/etc/bashrc\"" | tee -a $HOME/.bashrc $HOME/.zshrc

# echo -e '{\n "registry-mirrors": ["https://registry.docker-cn.com"] \n}' | sudo tee /etc/docker/daemon.json
# curl -s https://api.github.com/repos/gorhill/uBlock/releases/latest | grep "uBlock*" | cut -d : -f 2,3 | tr -d \" | wget -qi - ; rm -rf $HOME/Downloads/*,

<< EOF
# ForARM
exportproxy && for iAPK in {\
"org.mozilla.firefox_beta",\
"com.apkpure.installer",\
"org.videolan.vlc",\
"com.jacksoftw.webcam",\
"com.lonelycatgames.Xplore",\
"com.google.android.apps.translate",\
"com.google.android.inputmethod.latin",\
"com.tencent.mm",\
"com.bilibili.app.in",\
"com.whatsapp",\
"com.microsoft.teams",\
"com.tranzmate"\
"it.kyntos.webus"\
}; do docker run --rm -v $HOME/Downloads:/output ghcr.io/efforg/apkeep:stable -a $iAPK /output && adb install "$iAPK.apk"; done
# ForVPS
sudo su
apt update -y
apt install iptables-persistent wget curl -y
iptables -I INPUT -s 0.0.0.0/0 -p tcp --match multiport --dports 22,80,443 -j ACCEPT
iptables-save
netfilter-persistent save
netfilter-persistent reload
iptables -L -n --line-numbers
EOF
