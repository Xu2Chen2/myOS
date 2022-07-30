#!/bin/bash

sudo tee /etc/sudoers.d/$USER <<< "$USER ALL=(ALL) NOPASSWD: ALL"
sudo chmod 440 /etc/sudoers.d/$USER
sed -i '30,33d' $HOME/.config/powermanagementprofilesrc
sed -i '18,25d' $HOME/.config/powermanagementprofilesrc
sed -i '3,9d' $HOME/.config/powermanagementprofilesrc
echo -e "[Daemon]\nAutolock=false\nLockOnResume=false" | tee $HOME/.config/kscreenlockerrc
sudo pacman-mirrors --geoip -m rank
geoLocation=$(pacman-mirrors --status | grep -c China)
sudo pamac remove --no-confirm matray
sudo pamac checkupdates -a
sudo pamac upgrade -a --force-refresh --no-confirm
sudo pamac install --no-confirm archlinux-keyring
balooctl suspend
balooctl disable
echo "alias mountsdb=\"mkdir $HOME/sdb; sudo mount /dev/sdb1 $HOME/sdb; sudo chmod -R 777 $HOME/sdb\"" | tee -a $HOME/.bashrc $HOME/.zshrc
echo "alias umountsdb=\"sudo umount -v /dev/sdb1\"" | tee -a $HOME/.bashrc $HOME/.zshrc
source $HOME/.bashrc
source $HOME/.zshrc
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
user_pref(\"network.proxy.socks\", \"127.0.0.1\");\n\
user_pref(\"network.proxy.socks_port\", 7890);\n\
user_pref(\"network.proxy.socks_remote_dns\", true);\n\
user_pref(\"media.ffmpeg.vaapi.enabled\", true);\n\
user_pref(\"media.ffvpx.enabled\", false);\n\
user_pref(\"media.rdd-vpx.enabled\", false);\n\
user_pref(\"media.av1.enabled\", false);\n\
user_pref(\"browser.startup.homepage\", \"about:blank\");\n\
" | tee $ffUserRelease/user.js

sudo pamac install --no-confirm qt5-3d qt5-base qt5-charts qt5-connectivity qt5-datavis3d qt5-declarative qt5-doc qt5-examples qt5-gamepad qt5-graphicaleffects qt5-imageformats qt5-location qt5-lottie qt5-multimedia qt5-networkauth qt5-purchasing qt5-quick3d qt5-quickcontrols qt5-quickcontrols2 qt5-quicktimeline qt5-remoteobjects qt5-script qt5-scxml qt5-sensors qt5-serialbus qt5-serialport qt5-speech qt5-svg qt5-tools qt5-translations qt5-virtualkeyboard qt5-wayland qt5-webchannel qt5-webengine qt5-webglplugin qt5-websockets qt5-webview qt5-x11extras qt5-xmlpatterns
sudo pamac install --no-confirm texstudio texlive-core texlive-bibtexextra texlive-fontsextra texlive-formatsextra texlive-latexextra texlive-pictures texlive-pstricks texlive-music texlive-publishers texlive-science texlive-humanities texlive-games texlive-langchinese texlive-langjapanese texlive-langkorean texlive-langextra
sudo pamac install --no-confirm libxau libxi libxss libxtst libxcursor libxcomposite libxdamage libxfixes libxrandr libxrender mesa-libgl alsa-lib libglvnd
sudo pamac install --no-confirm ruby perl-image-exiftool gitlab-workhorse gitlab-gitaly redis gitlab-shell gitlab yarn
sudo pamac install --no-confirm fcitx5 fcitx5-configtool fcitx5-gtk fcitx5-qt fcitx5-chinese-addons
sudo pamac install --no-confirm gvfs-smb samba kdenetwork-filesharing manjaro-settings-samba
sudo pamac install --no-confirm base-devel ninja openmpi tbb cmake make python python-numpy
sudo pamac install --no-confirm wine winetricks wine-mono wine_gecko
sudo pamac install --no-confirm libxcrypt-compat
sudo pamac install --no-confirm intel-gpu-tools
sudo pamac install --no-confirm android-tools
sudo pamac install --no-confirm net-tools
sudo pamac install --no-confirm glfw-x11
sudo pamac install --no-confirm gconf
sudo pamac install --no-confirm nmap
sudo pamac install --no-confirm barrier
sudo pamac install --no-confirm git
sudo pamac install --no-confirm wget
sudo pamac install --no-confirm zip
sudo pamac install --no-confirm unzip
sudo pamac install --no-confirm vlc
sudo pamac install --no-confirm octave
sudo pamac install --no-confirm freerdp
sudo pamac install --no-confirm remmina
sudo pamac install --no-confirm libreoffice
sudo pamac install --no-confirm kdenlive
sudo pamac install --no-confirm gimp
sudo pamac install --no-confirm inkscape
sudo pamac install --no-confirm audacity
sudo pamac install --no-confirm obs-studio
sudo pamac install --no-confirm flameshot
sudo pamac install --no-confirm rnote
sudo pamac install --no-confirm wireguard-tools
sudo pamac install --no-confirm firefox-developer-edition
sudo pamac install --no-confirm filezilla
sudo pamac install --no-confirm paraview
sudo pamac install --no-confirm yt-dlp you-get
sudo pamac install --no-confirm pandoc
sudo pamac install --no-confirm chromium && xdg-settings set default-web-browser chromium.desktop
sudo pamac install --no-confirm syncthing && sudo systemctl enable --now syncthing@$USER

sudo pamac install --no-confirm clash
clash -t
echo "alias exportproxy=\"export http_proxy=http://127.0.0.1:7890/ ; export https_proxy=https://127.0.0.1:7890/ ; export ftp_proxy=http://127.0.0.1:7890/\"" | tee -a $HOME/.bashrc $HOME/.zshrc
source $HOME/.bashrc
source $HOME/.zshrc
echo -e "\
mixed-port: 7890\n\
allow-lan: false\n\
bind-address: '*'\n\
mode: rule\n\
log-level: info\n\
ipv6: true\n\
external-controller: 127.0.0.1:9090\n\
external-ui: clash-dashboard\n\
secret: '0000'\n\
dns:\n\
  enable: true\n\
  listen: 0.0.0.0:1053\n\
  ipv6: true\n\
  default-nameserver:\n\
   - 1.0.0.1\n\
   - 208.67.222.222\n\
  nameserver:\n\
   - https://i.passcloud.xyz/dns-query\n\
   - https://a.passcloud.xyz/dns-query\n\
  enhanced-mode: fake-ip\n\
  fake-ip-range: 198.18.0.1/16\n\
  fallback:\n\
   - https://cloudflare-dns.com/dns-query\n\
  fallback-filter:\n\
   geoip: true\n\
   geoip-code: CN\n\
rules:\n\
  - DOMAIN-SUFFIX,cn,DIRECT\n\
  - GEOIP,CN,DIRECT,no-resolve\n\
  - MATCH,tSG\n\
proxies:\n\
  - {name: \"sDE\", type: ss, cipher: chacha20-ietf-poly1305, port: 00000, server: 0.0.0.0, password: 0}\n\
  - {name: \"sSG\", type: ss, cipher: chacha20-ietf-poly1305, port: 00000, server: 0.0.0.0, password: 0}\n\
  - {name: \"tDE\", type: trojan, port: 443, server: c.c, password: 0}\n\
  - {name: \"tSG\", type: trojan, port: 443, server: c.c, password: 0}\n\
  - {name: \"vJC\", type: vmess, alterId: 0, cipher: none, port: 443, server: 0.0.0.0, uuid: 0}\n\
proxy-groups:\n\
  - name: \"relay\"\n\
    type: relay\n\
    proxies:\n\
      - vJC\n\
      - sSG\n\
" | tee $HOME/.config/clash/config.yaml
if [ "$geoLocation" -gt 0 ]; then
    git clone -b gh-pages --depth 1 https://hub.fastgit.xyz/Dreamacro/clash-dashboard $HOME/.config/clash/clash-dashboard
else
    git clone -b gh-pages --depth 1 https://github.com/Dreamacro/clash-dashboard $HOME/.config/clash/clash-dashboard
fi

sudo pamac install --no-confirm docker
sudo systemctl daemon-reload
sudo systemctl enable docker
sudo usermod -aG docker $USER
sudo systemctl start docker
sudo pamac install --no-confirm docker-compose
if [ "$geoLocation" -gt 0 ]; then
    echo -e '{\n "registry-mirrors": ["https://registry.docker-cn.com","http://hub-mirror.c.163.com","https://docker.mirrors.ustc.edu.cn"] \n}' | sudo tee /etc/docker/daemon.json && sudo systemctl restart docker
fi

sudo pamac install --no-confirm virt-manager qemu vde2 dnsmasq bridge-utils openbsd-netcat edk2-ovmf swtpm
yes | sudo pamac install --no-confirm iptables-nft
sudo systemctl daemon-reload
sudo systemctl enable libvirtd
sudo usermod -aG kvm $USER
sudo usermod -aG libvirt $USER
sudo systemctl start libvirtd

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
pip install pystun3 numpy pandas matplotlib scienceplots gmsh

wget -P $HOME/Downloads https://gmsh.info/bin/Linux/gmsh-stable-Linux64.tgz
tar -zxvf $HOME/Downloads/gmsh-stable-Linux64.tgz -C $HOME/Downloads
cp -f $HOME/Downloads/gmsh-4.10.5-Linux64/bin/gmsh $HOME/
echo "alias gmsh=\"$HOME/gmsh\"" | tee -a $HOME/.bashrc $HOME/.zshrc
source $HOME/.bashrc
source $HOME/.zshrc

wget -P $HOME/Downloads https://dl.openfoam.com/source/v2112/OpenFOAM-v2112.tgz
wget -P $HOME/Downloads https://dl.openfoam.com/source/v2112/ThirdParty-v2112.tgz
mkdir $HOME/openfoam
tar -zxvf $HOME/Downloads/OpenFOAM-v2112.tgz -C $HOME/openfoam/
tar -zxvf $HOME/Downloads/ThirdParty-v2112.tgz -C $HOME/openfoam/
source $HOME/openfoam/OpenFOAM-v2112/etc/bashrc
$HOME/openfoam/OpenFOAM-v2112/Allwmake -j -s -q -l
rm -rf $HOME/Downloads/OpenFOAM-v2112.tgz $HOME/Downloads/ThirdParty-v2112.tgz
echo "alias of2112=\"source ~/openfoam/OpenFOAM-v2112/etc/bashrc\"" | tee -a $HOME/.bashrc $HOME/.zshrc
source $HOME/.bashrc
source $HOME/.zshrc

sudo sed -i 's/#EnableAUR/EnableAUR/g' /etc/pamac.conf
sudo pamac install --no-confirm wps-office ttf-wps-fonts wps-office-mui-zh-cn wps-office-fonts ttf-ms-fonts wps-office-all-dicts-win-languages
sudo sed -i '2a \
export XMODIFIERS="@im=fcitx"\
export QT_IM_MODULE="fcitx"' /usr/bin/wps /usr/bin/et /usr/bin/wpp /usr/bin/wpspdf
sudo pamac install --no-confirm debtap
sudo debtap -u

sudo pamac clean -k 0 -b -v --no-confirm; sudo pamac clean -b -v --no-confirm; yes | sudo pacman -Scc
# wget -P $HOME/Downloads https://az792536.vo.msecnd.net/vms/VMBuild_20190311/VirtualBox/MSEdge/MSEdge.Win10.VirtualBox.zip && unzip -d $HOME/Downloads $HOME/Downloads/MSEdge.Win10.VirtualBox.zip && tar -xvf $HOME/Downloads/'MSEdge - Win10.ova' -C $HOME/Downloads/ && qemu-img convert -O qcow2 $HOME/Downloads/'MSEdge - Win10-disk001.vmdk' $HOME/win10stable1809.qcow2 && rm -rf $HOME/Downloads/*.*
