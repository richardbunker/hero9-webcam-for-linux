# Hero9 Webcam for Linux Guide
Idea inspired by Adam Lynch -> https://www.youtube.com/watch?v=gS9FqDqWFAE&ab_channel=AdamLynch and after a few hours was able to get something up and running. Please help to improve this method 🙏

# My System
Pop!_os 20.10
Dell XPS 15

# Software Dependencies
`v4l2loopback-utils v4l2loopback-dkms ffmpeg`

# Instructions

1. Ensure HERO9 "USB Connection" is set to "GoPro Connect" in the *Connections* menu.

2. Connect via usb to your computer.

3. Open Network settings, and enable the new USB ethernet interface

<img src=https://github.com/richardbunker/hero9-webcam-for-linux/blob/main/Network.png></img>

4. Find the your computer's IP on the new network via `ifconfig`

<img src=https://github.com/richardbunker/hero9-webcam-for-linux/blob/main/ifconfig.png></img>

5. Find the GoPro's IP in the network
    - eg my Computer's IP was 172.24.106.53 and the GoPro's IP was 172.24.106.51

6. To fire up the webcam mode on the GoPro hit URL `http://<YOUR_GOPRO_IP_HERE>/gp/gpWebCam/START?res=1080` in a browser.
7. GoPro will send JSON response `{"Status": 2, "Error": 0}`
8. `sudo modprobe v4l2loopback exclusive_caps=1 max_buffers=2`
9. Pipe your GoPro's UDP stream on port 8554 to ffmpeg and set the output as and new video device (/dev/video4) `ffmpeg -i udp://<<YOUR_GOPRO_IP_HERE>:8554 -vcodec rawvideo -pix_fmt yuv420p -threads 0 -f v4l2 /dev/video4`
    - /dev/video4 seems to work for me
10. Open OBS and test it out
11. To stop the webcam mode hit URL `http://<YOUR_GOPRO_IP_HERE>/gp/gpWebCam/STOP` in a browser.
