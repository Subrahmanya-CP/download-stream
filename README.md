# download-stream
# -------------------------------
# FFmpeg Download Stream Cheat Sheet
# -------------------------------

prerequisites:
  - ffmpeg installed
    linux: 
      - sudo apt update
      - sudo apt install ffmpeg
    windows: 
      - download from https://ffmpeg.org/download.html
      - add ffmpeg.exe to PATH
  - stream URL from Ant Media Server
    examples:
      - RTMP: rtmp://192.168.43.6/live/mp4test
      - HLS: http://192.168.43.6:5080/LiveApp/streams/mp4test.m3u8
  - output directory for saving files

commands:
  # 1. Download RTMP Stream
  download_rtmp: >
    ffmpeg -i rtmp://192.168.43.6/live/mp4test -c copy output.mp4

  # 2. Download HLS Stream
  download_hls: >
    ffmpeg -i http://192.168.43.6:5080/LiveApp/streams/mp4test.m3u8 -c copy output.mp4

  # 3. Download with fixed duration (e.g., 60 seconds)
  download_duration: >
    ffmpeg -i rtmp://192.168.43.6/live/mp4test -c copy -t 60 output.mp4

alternatives:
  - FFmpeg: "Fast, free, command-line, reliable for live streams"
  - Streamlink: "CLI tool, good for HLS/HTTP streams"
  - VLC: "GUI, open network stream → Convert/Save"
  - OBS Studio: "GUI, can record live streams locally"
  - Browser Extensions: "Video DownloadHelper, Flash Video Downloader"

best_practices:
  - always use '-c copy' to avoid re-encoding if format not needed
  - ensure sufficient disk space for long streams
  - start recording early to capture live content fully
  - test short streams first to verify URL and playback

example_workflow:
  1: "Start Ant Media Server in VM"
    command: |
      cd /usr/local/antmedia
      sudo ./start.sh
  2: "Start FFmpeg on host OS to stream to VM"
    command: >
      ffmpeg -re -i rtmp://192.168.43.6/live/mp4test -c copy output.mp4
  3: "Open browser and play stream"
     url: "http://192.168.43.6:5080/LiveApp/play.html?name=mp4test"
  4: "Stop recording when done (Ctrl+C)"
  5: "Verify output.mp4 plays correctly"

notes:
  - AMS does not provide direct download
  - RTMP may buffer and hide packet loss
  - WebRTC playback shows real-time congestion better
