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
