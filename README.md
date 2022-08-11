# Cordova Streaming Media plugin

For iOS and Android, by [Nicholas Hutchind](https://github.com/nchutchind)

## Description

This plugin allows you to stream audio and video in a fullscreen, native player on iOS and Android.

* 1.0.0 Works with Cordova 3.x
* 1.0.1+ Works with Cordova >= 4.0

## Message from the maintainer:

I no longer contribute to Cordova or Ionic full time. If your org needs work on this plugin please consider funding it and hiring me for improvements or otherwise consider donating your time and submitting a PR for whatever you need fixed. My contact info can be found [here](https://github.com/shamilovtim). 

## Installation

```
cordova plugin add https://github.com/nchutchind/cordova-plugin-streaming-media
```

### iOS specifics
* Uses the AVPlayerViewController
* Tested on iOS 12 or later

### Android specifics
* Uses VideoView and MediaPlayer.
* Creates two activities in your AndroidManifest.xml file.
* Tested on Android 4.0+

## Usage

```javascript
  var videoUrl = STREAMING_VIDEO_URL;

  // Just play a video
  window.plugins.streamingMedia.playVideo(videoUrl);

  // Play a video with callbacks
  var options = {
    successCallback: data => {
      if(data == 'OK') {
        console.log("Video was closed without error.");
      } else {
        handleCallback(data);
      }
    },
    errorCallback: errMsg => console.log("Error! " + errMsg),
    orientation: 'landscape',
    shouldAutoClose: true,  // true(default)/false
    controls: true, // true(default)/false. Used to hide controls on fullscreen
    headers: {
      Authorization: `Basic ${btoa(username + ':' + password)}`
    },
    duration: 30000 // Optional custom duration in milliseconds (Android only)
  };

  function handleCallback(data) {
    switch(data.eventType) {
        case 'PLAY':
            console.log('Video is playing');
            break;
        case 'PAUSE':
            console.log('Video is paused');
            break;
        case 'SEEK':
            console.log(`Seeking video to ${data.msec}`);
            break;
        case 'LIFECYCLE_ONPAUSE': // Android only
            console.log('Activity paused');
            break;
        case 'LIFECYCLE_ONRESUME': // Android only
            console.log('Activity resumed');
            break;
        default:
            console.error("Unhandled stream event callback", data);
    }

  window.plugins.streamingMedia.playVideo(videoUrl, options);

  // Stop current video
  window.plugins.streamingMedia.stopVideo();

  var audioUrl = STREAMING_AUDIO_URL;

  // Play an audio file (not recommended, since the screen will be plain black)
  window.plugins.streamingMedia.playAudio(audioUrl);

  // Play an audio file with options (all options optional)
  var options = {
    bgColor: "#FFFFFF",
    bgImage: "<SWEET_BACKGROUND_IMAGE>",
    bgImageScale: "fit", // other valid values: "stretch", "aspectStretch"
    initFullscreen: false, // true is default. iOS only.
    keepAwake: false, // prevents device from sleeping. true is default. Android only.
    successCallback: function() {
      console.log("Player closed without error.");
    },
    errorCallback: function(errMsg) {
      console.log("Error! " + errMsg);
    }
  };
  window.plugins.streamingMedia.playAudio(audioUrl, options);

  // Stop current audio
  window.plugins.streamingMedia.stopAudio();

  // Pause current audio (iOS only)
  window.plugins.streamingMedia.pauseAudio();

  // Resume current audio (iOS only)
  window.plugins.streamingMedia.resumeAudio();  

```

## Special Thanks

[Michael Robinson (@faceleg)](https://github.com/faceleg)

[Timothy Shamilov (@shamilovtim)](https://github.com/shamilovtim)
