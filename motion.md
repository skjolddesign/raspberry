## Ip camera
- This is a complete walkthroug to turn your Armbian and usb camera to a ip camera with Motion.
- Tested on Orange Pi Zero with image: Armbian_5.90_Orangepizero_Ubuntu_bionic_next_4.19.57.img.
- Source info:  https://www.instructables.com/How-to-Make-Raspberry-Pi-Webcam-Server-and-Stream-/
- After you are done you find camera on port 8080 or 8081.

## Setup
- Install os and login to your user.
```
sudo apt-get update
sudo apt-get install motion
```
Check if your usb cam is found:
```
lsusb
```
Configure motion:
```
sudo nano /etc/motion/motion.conf
```
set variables:
- daemon on
- log_level 4
- power_line_frequency 1
- width 640
- height 480
- framerate 2
- auto_brightness on
- threshold 15000
- output_pictures off
- ffmpeg_output_movies off
- text_left Motion %t
- text_double on
- stream_maxrate 2
- stream_localhost off
- webcontrol_localhost off

After changes in motion.conf you need to restart service and start motion manualy:
```
sudo service motion restart
```
Run:
```
sudo motion
```
Set motion to owner of log folder if you get log permission error:
```
sudo chown motion:motion /var/log/motion
```
## Run on startup
```
sudo nano /etc/default/motion
```
- Set ' start_motion_daemon ' to yes.
```
sudo systemctl enable motion
sudo service motion start
sudo service motion status
sudo service motion stop
reboot
```
if you need to read log:
```
sudo cat /var/log/motion/motion.log
```

