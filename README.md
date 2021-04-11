# SettingUpUbuntu
Collection of initial setup steps

### Adding a user group and editing writes for /opt/ folder

Help from: https://www.howtogeek.com/50787/add-a-user-to-a-group-or-second-group-on-linux/

**Add user group and current user to that group **

```bash
sudo groupadd LaptopGroup
sudo usermod -a -G LaptopGroup myuser
```

Log out and back in to make the changes effective

**Give permissions to /opt folder**

```bash
sudo chgrp LaptopGroup /opt/
sudo chmod -R 775 /opt/
```

#### Fix Wifi not connecting issue (disable powersave)

Help from: https://ubuntuforums.org/showthread.php?t=2396357&page=2

```bash
sudo sed -i 's/wifi.powersave = 3/wifi.powersave = 2/' /etc/NetworkManager/conf.d/default-wifi-powersave-on.conf
systemctl restart network-manager.service
```

#### Install Opera

Help from: https://linuxconfig.org/how-to-install-opera-browser-on-ubuntu-18-04-bionic-beaver-linux

```bash
wget -qO- https://deb.opera.com/archive.key | sudo apt-key add -
sudo add-apt-repository "deb [arch=i386,amd64] https://deb.opera.com/opera-stable/ stable non-free"

sudo apt install opera-stable
```

#### Configure Opera to watch HTML5 videos

Help from: https://github.com/devkicks/OperaFixUbuntu18

```
# install chrome and extra ffmpeg for chrome
sudo apt install chromium-codecs-ffmpeg-extra chromium-browser

# backup old ffmpeg in opera folder
sudo mv /usr/lib/x86_64-linux-gnu/opera/libffmpeg.so /usr/lib/x86_64-linux-gnu/opera/libffmpeg.BACKUP

# copy over from chrome
sudo cp /usr/lib/chromium-browser/libffmpeg.so /usr/lib/x86_64-linux-gnu/opera/.

# change to executable
sudo chmod +x /usr/lib/x86_64-linux-gnu/opera/libffmpeg.so
```

#### Other settings

- **Configure adblocker and remove allowed sites**
- **Remove extra bloatware sites from speed dial**
- **Settings->Dock->Icon Size --> 24**

#### Install Useful packages, Python and Virtualenv

```bash
sudo apt install python-dev python-pip python3-dev python3-pip virtualenv vim-gnome tmux gedit-plugins
```

##### Setting up virtualenv with TF cpu

``` bash
mkdir /opt/virtual_env
virtualenv /opt/virtual_env/venv_p2
virtualenv -p python3 /opt/virtual_env/venv_p3

source /opt/virtual_env/venv_p2/bin/activate
pip install tensorflow
deactivate

source /opt/virtual_env/venv_p3/bin/activate
pip install tensorflow
pip3 install spyder
deactivate
```

#### Set up bash_aliases

```bash
vim ~/.bash_aliases
```

copy the following text

```bash
# ~/.bash_aliases
alias venv_p3="source /opt/virtual_env/venv_p3/bin/activate"
alias venv_p2="source /opt/virtual_env/venv_p2/bin/activate"
alias sshm1="ssh -t mua@sshm1 'bash' "
alias de="deactivate"
alias ptv="python -c 'import torch; print(torch.__version__); print(torch.cuda.is_available())'"
alias tfv="python -c 'import tensorflow as tf; print(tf.__version__); print(tf.test.is_gpu_available())'"

# add anything other alias here

# print out
echo "###########################"
echo "Setting up aliases"
echo "venv_p3: Python3 virtualenv"
echo "venv_p2: Python2 virtualenv"
echo "sshmx: ssh to sshmx machine"
echo "###########################"

```

#### Setting up ~/.vimrc

```bash
vim ~/.vimrc
```

Paste the following:

````bash
set tabstop=4 shiftwidth=4 expandtab
set autoindent
````

#### Setting up gedit

- Menu-> Preferences -> Plugins
- Enable:
  - Code comment
  - Dashboard
  - Multi edit
  - Word completion
- Menu -> Preferences -> View
  - Display line numbers
  - Display right margin at column: 80
  - Display overview map
  - Highlight current line
  - Highlight matching brackets
- Menu -> Preferences -> Editor
  - Tab width: 4
  - Insert spaces instead of tabs
  - Enable automatic indentation
- Menu -> Preferences -> Font and Colours
  - Solarized Dark

#### Setup fstab for automount existing drives
````bash
sudo vim /etc/fstab
````

````bash
UUID=xxxxxxxxxxxxxxxxxxxxxx /               ext4    errors=remount-ro 0       1

UUID=xxxxxxxxxxxxxxxxxxxxxx /data1          ext4    defaults        0       2

UUID=xxxxxxxxxxxxxxxxxxxxxx /data2          ext4    defaults        0       2

UUID=xxxxxxxxxxxxxxxxxxxxxx none            swap    sw              0       0
````
### Install VSCODE and configure for python
- Download latest debian package from: https://code.visualstudio.com/
- Install using the dpkg as follows:
````bash
sudo dpkg -i code_*.deb
````
#### Setting virutalenv folders for list of interpreters for VSCODE
Often on a prototyping workstation, it is helpful to use a single testing virtualenv as it speeds up setting up things.
To make a default virtualenv accessing in all new projects, update the following field in your user settings.json
- Go to settings **ctrl+,**
- Open **settings.json** from top right corner file icon
- Append json with entry
  ```` bash
  "python.pythonPath": "/opt/virtualenvs/venv/bin/python3"
  ````
- Save and reload

#### Setting virutalenv folders for list of interpreters for VSCODE
- Go to settings **ctrl+,**
- Search **virtualenv**
- Write path **/opt/virtual_env**
- Save

#### Useful VSCode extensions
- python extension: ms-python.python
- remote development: ms-vscode-remote.vscode-remote-extensionpack
- bracket pair coloriser: coenraads.bracket-pair-colorizer-2

#### Nice to have VSCode extensions
- material icons: pkief.material-icon-theme
- reload: natqe.reload
- resource monitor: mutantdino.resourcemonitor
- nvidia-smi: innerlee.nvidia-smi

### Thinkvantage button reprogram
Specific to Thinkpad laptop

Help from: https://askubuntu.com/a/561464

Install:
````bash
sudo apt-get install xbindkeys 
````

Then run:
````bash
xbindkeys -k  
````

and press the thinkvantage (or any other button you wish to reprogram). The following should appear:

````bash
Press combination of keys or/and click under the window.
You can use one of the two lines after "NoCommand"
in $HOME/.xbindkeysrc to bind a key.
"NoCommand"
    m:0x0 + c:159
    NoSymbol  
````

We will use the key id from this.

Create ~/.xindkeysrc  
````bash
touch ~/.xindkeysrc  
````

Edit the file and add the following:
````bash
"bash /path/to/your/script.sh"
m:0x0 + c:159 
````

### Setting up terminal to not show pop-up 'close this terminal?'
https://askubuntu.com/a/999781

In 16.04 you'll find that choice in dconf-editor. You can install it, if necessary, with
````
sudo apt install dconf-editor
````
Then go to
````
org -> gnome -> terminal -> legacy
````
and uncheck ````confirm-close````.

### Setting up shortcut keys for Spotify
https://askubuntu.com/questions/1105363/spotify-keyboard-controls-not-working
````
#!/usr/bin/env bash

# Simple Spotify Control
# Just call ./spotify_control --help

CMD="dbus-send --print-reply --dest=org.mpris.MediaPlayer2.spotify /org/mpris/MediaPlayer2 org.mpris.MediaPlayer2.Player"

case "$1" in
  "playpause" )
    ${CMD}.PlayPause
    exit $?
  ;;
  "next" )
    ${CMD}.Next
    exit $?
  ;;
  "previous" )
    ${CMD}.Previous
    exit $?
  ;;
  "stop" )
    ${CMD}.Stop
    exit $?
  ;;
  "play" )
    ${CMD}.Play
    exit $?
  ;;
  *)
    echo "Usage: $0 [command]"
    echo "  commands are: playpause, next, previous, stop, play"
    exit 1
  ;;
esac
````
Go to settings -> keyboard -> keyboard shortcuts -> custom shortcuts

Commands available:
````
/home/your_username/Downloads/spotify_control playpause
/home/your_username/Downloads/spotify_control previous
/home/your_username/Downloads/spotify_control next
/home/your_username/Downloads/spotify_control play
/home/your_username/Downloads/spotify_control stop
````
### Setting up launcher shortcut for manually installed apps
- Place you app folder inside /opt e.g. /opt/RealVNC/*
- Create a .desktop file with application name e.g. RealVNC.desktop and add the following fields:
  ````
  [Desktop Entry]
  Categories=Connectivity;Remote;Desktop;VPN;VNC;
  Exec=/opt/RealVNC/VNC-Viewer-6.20.113-Linux-x64
  GenericName=RealVNC
  Comment=RealVNC Remote Desktop Viewer
  Icon=/opt/RealVNC/icon.png
  Keywords=connect;remote;desktop;vnc;vpn;
  Name=RealVNC
  StartupNotify=true
  Terminal=false
  Type=Application
  ````
- Place the .desktop file in folder ~/.local/share/applications/*

### Port forwarding for applications/tensorboard from remote server to local
On you local machine login to an ssh session using the following:

````
ssh -L localhost:16006:localhost:6006 user@host 
````
The above forwards port 6006 from host to port 16006 on your local machine. To access your tensorboard/application open: http://localhost:16006 on your local machine. Help from: https://stackoverflow.com/questions/37987839/how-can-i-run-tensorboard-on-a-remote-server/42445070#42445070


### Setting up CUDA TOOLKIT
- Goto: https://developer.nvidia.com/cuda-10.2-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal  and download the local .run file for your OS (Ubuntu 16 in my case)
- Add execute permission: ````chmod +x cuda_10.2.run````
- Run with following options to install locally to your folder of choice (no drivers):
````
./cuda_10.2.89_440.33.01_linux.run --silent --toolkit --toolkitpath=/mnt/shared/muhammad/opt/nvidia/cuda-10.2 --defaultroot=/mnt/shared/muhammad/opt/nvidia/cuda-10.2
````
Help from: https://forums.developer.nvidia.com/t/failing-to-install-10-1-via-run-file-on-rhel7-as-non-root/72087/6


# SettingUpWSL2Ubuntu

### Stop bell sound in terminal
````
echo 'set bell-style none' >> /etc/inputrc
````
### WSL2 setup with Ubuntu GUI
https://gist.github.com/tdcosta100/385636cbae39fc8cd0937139e87b1c74
