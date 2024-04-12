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

Run a video file

```bash
gst-launch-1.0 -e filesrc location=video.mp4 \
! qtdemux \
! h265parse \
! nvv4l2decoder \
! nvvideoconvert \
! nvoverlaysink
```

# OpenCV

## Get frames from video and then spit them back out to screen. No X11 etc involved
```python
import cv2


# Input
# Note if the jetson can't keep up, use `appsink drop=1` to drop frames and just parse the ones you can
source = "filesrc location=video.mp4 ! qtdemux ! h265parse ! nvv4l2decoder ! nvvideoconvert ! video/x-raw, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink"
cap = cv2.VideoCapture(source, cv2.CAP_GSTREAMER)   



# Output to screen
gst_out = "appsrc ! video/x-raw, format=BGR ! videoconvert ! video/x-raw, format=BGRx ! videoconvert !  nvvideoconvert ! video/x-raw(memory:NVMM), format=I420 ! nvoverlaysink sync=0"
out = cv2.VideoWriter(gst_out, cv2.CAP_GSTREAMER, 0, 20, (3840,2160)) 

                                                                                                                                                                                          
while cap.isOpened():

    # Get frame from input                                        
    ret, frame = cap.read()
    
                                                                                                                                                                                         
    if not ret:                                                      
        print("Failed to grab frame")     
        break  
        
    # Write frame to output (screen in our case)    
    out.write(frame)
        
        
out.release()                                                                                                                    
cap.release() 
```
