## CHANGE-FOR-FORK

[![](https://jitpack.io/v/dimadesu/LibRtmp-Client-for-Android.svg)](https://jitpack.io/#dimadesu/LibRtmp-Client-for-Android)


The configuration of target_link_libraries in [CMakeLists.txt](rtmp-client/CMakeLists.txt) has been modified to support the requirements of [Support 16 KB page sizes](https://developer.android.com/guide/practices/page-sizes).

### about androidx.media3:media3-datasource-rtmp

androidx.media3 [RTMP DataSource module](https://github.com/androidx/media/tree/release/libraries/datasource_rtmp) uses [the rtmp-client library](https://github.com/ant-media/LibRtmp-Client-for-Android), but the upstream developers have not updated it in time to support 16k, if you encounter [the same problem](https://github.com/androidx/media/issues/1918), you can try use this library first to solve the problem.

![Image](https://github.com/user-attachments/assets/f6b76cc5-e2b4-4157-83f7-f0f84e0a80b4)

```kts
  implementation("androidx.media3:media3-datasource-rtmp:1.6.1") {
    exclude("io.antmedia", "rtmp-client")
  }
  implementation("com.github.dimadesu:LibRtmp-Client-for-Android:v3.2.0-16kb-ndk-r28c-v2")
```

---

# Librtmp Client for Android
It is probably the smallest(~60KB) rtmp client for android. It calls librtmp functions over JNI interface.
With all cpu architectures(arm, arm7, arm8, x86, x86-64, mips) its size is getting about 300KB

It compiles librtmp library without ssl. 

In version 0.2, it supports FLV muxing and sending stream via RTMP. FLV muxing is based on this repo https://github.com/rainfly123/flvmuxer


##### To read streams, you can call below functions of **RtmpClient** from Java #####

For live streams add **" live=1"** at the end of the url when calling the **_open_** function

* *public native int open(String url, boolean isPublishMode);*
* *public native int read(byte[] data, int offset, int size);*
* *public native int write(byte[] data);*
* *public native int seek(int seekTime);*
* *public native int pause(int pause);*
* *public native int close();*
* *public native int isConnected();*: Call this function to query the connection status. Returns 1 if connected, returns 0 if not connected

Don't forget calling the **_close_** function after you are done. If you don't, there will be memory leakage


##### To publish streams, you can call below functions of **RtmpMuxer** from Java #####
* *public native int open(String url, int width, int height);* : First, call this function with the url you plan to publish. Width and height are the width and height of the video. These two parameters are not mandatory. They are optional. They put width and height values into the metadata tag.
* *public native int writeVideo(byte[] data, int offset, int length, int timestamp);*: Write h264 nal units with this function
* *public native int writeAudio(byte[] data, int offset, int length, int timestamp);*: Write aac frames with this function
* *public native int close();*: Call this function to close the publishing.
* *public native int isConnected();*: Call this function to query the connection status. Returns 1 if connected, returns 0 if not connected

To save flv file locally as well, you can use below functions
* *public native void file_open(String filename);* : call this function with full file path
* *public native void write_flv_header(boolean is_have_audio, boolean is_have_video);*: After opening file call this function
* *public native void file_close();*: Call this function to close the file. 

if any local file is opened, library will write the audio and video frames to local file as well. 

## Install ##

- Add dependency to your build gradle
```sh
dependencies {
    ...
    compile 'net.butterflytv.utils:rtmp-client:3.1.0'
    ...
}
```

- That's all. You can use RtmpClient class


<br/>

[Ant Media](http://antmedia.io)

