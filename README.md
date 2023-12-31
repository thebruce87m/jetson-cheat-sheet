# jetson-cheat-sheet


## Remove unwanted applications
```bash
sudo apt-get purge libreoffice* thunderbird rhythmbox aisleriot gnome-mahjongg gnome-mines gnome-sudoku shotwell
sudo apt-get autoremove
sudo apt-get autoclean
```

# Upgrade pip

```bash
sudo apt install python3-pip
sudo pip3 install --upgrade pip
```

# Install jtop

```bash
sudo -H pip3 install -U jetson-stats
```

# Configure Docker

Edit /etc/docker/daemon.json
```bash
{​​​​​​​
    "runtimes": {​​​​​​​
        "nvidia": {​​​​​​​​​​​​​​
            "path": "nvidia-container-runtime",
            "runtimeArgs": []
        }​​​​​​​​​​​​​​
    }​​​​​​​​​​​​​​,
    "default-runtime": "nvidia"
}​​​​​​​​​​​​​​
```

```bash
sudo systemctl restart docker.service
```

```bash
sudo usermod -aG docker $USER
```

## Turn Off Screen Dim and Lock

Open "System Settings" and go to "Brightness & Lock"

    "Turn screen off when inactive for:" Never
    "Lock": OFF


## Install Deepstream

Gstreamer Dependencies

```bash
sudo apt install \
libssl1.1 \
libgstreamer1.0-0 \
gstreamer1.0-tools \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly \
gstreamer1.0-libav \
libgstreamer-plugins-base1.0-dev \
libgstrtspserver-1.0-0 \
libjansson4 \
libyaml-cpp-dev
```

```bash
# Find the version for the jetpack installed
apt search deepstream

# install it
sudo apt install deepstream-6.0
```
