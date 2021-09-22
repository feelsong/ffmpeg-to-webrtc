# ffmpeg-to-webrtc

ffmpeg-to-webrtc demonstrates how to send video from ffmpeg to your browser using [pion](https://github.com/pion/webrtc).

This example has the same structure as [play-from-disk-h264](https://github.com/pion/example-webrtc-applications/blob/master/play-from-disk-h264) but instead of reading from a file it reads H264 stream from ffmpeg stdout pipe.

## Instructions

### Open example page
[jsfiddle.net](https://jsfiddle.net/9s10amwL/) you should see two text-areas and a 'Start Session' button

### Copy browser's SessionDescription
In the jsfiddle the top textarea is your browser's SDP, copy that and:

#### Windows
1. Paste the SessionDescription into a file `SDP`.
2. Make sure ffmpeg in your PATH.
3. Run `go run . <ffmpeg command line options> - < SDP`
4. Note dash after ffmpeg options. It makes ffmpeg to write output to stdout. The app will read h264 stream from ffmpeg stdout.
5. ffmpeg output format should be h264. Browsers don't support all h264 profiles so it may not always work. Here is an example of format that works: `-pix_fmt yuv420p -c:v libx264 -bsf:v h264_mp4toannexb -b:v 2M -max_delay 0 -bf 0 -f h264`.

### Input SessionDescription from ffmpeg-to-webrtc into your browser
When you see SDP in base64 format printed it means that SDP is already in copy buffer. So you can go to jsfiddle page and paste that into second text area

### Hit 'Start Session' in jsfiddle
A video should start playing in your browser below the input boxes.

## Examples (windows)
### Share camera stream
```go run . -rtbufsize 100M -f dshow -i video="PUT_DEVICE_NAME" -pix_fmt yuv420p -c:v libx264 -bsf:v h264_mp4toannexb -b:v 2M -max_delay 0 -bf 0 -f h264 - < SDP```. 
There is a delay of several seconds. Should be possible to fix it with better ffmpeg configuration.

To check list of devices: `ffmpeg -list_devices true -f dshow -i dummy`.  
It is possible also to set a resolution and a format, for example `-pixel_format yuyv422 -s 640x480`.
Possible formats: `ffmpeg -list_options true -f dshow -i video=PUT_DEVICE_NAME`.
### Share screen or window
See `.bat` files in src folder
