<!--
# license: Licensed to the Apache Software Foundation (ASF) under one
#         or more contributor license agreements.  See the NOTICE file
#         distributed with this work for additional information
#         regarding copyright ownership.  The ASF licenses this file
#         to you under the Apache License, Version 2.0 (the
#         "License"); you may not use this file except in compliance
#         with the License.  You may obtain a copy of the License at
#
#           http://www.apache.org/licenses/LICENSE-2.0
#
#         Unless required by applicable law or agreed to in writing,
#         software distributed under the License is distributed on an
#         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#         KIND, either express or implied.  See the License for the
#         specific language governing permissions and limitations
#         under the License.
-->

# IMPORTANT
This plugin is already obsolete. They say that android 4.X already incorporates codecs for .aac+  
Besides, it has 32-bit libraries that I never recompiled.

# IMPORTANTE
 
Este plugin ya está obsoleto. Dicen que android 4.X ya incorpora codecs para .aac+  
Aparte tiene librerías en 32 bits que nunca recompilé. 

# cordova-plugin-mediaac


This plugin provides the ability to play back audio streaming on a device.
Support AAC, AAC+ and MP3 format.

__NOTE__: The current implementation does not adhere to a W3C
specification for media capture, and is provided for convenience only.
A future implementation will adhere to the latest W3C specification
and may deprecate the current APIs.

This plugin defines a global `Mediaac` Constructor.

Although in the global scope, it is not available until after the `deviceready` event.

```js
    document.addEventListener("deviceready", onDeviceReady, false);
    function onDeviceReady() {
        console.log(Mediaac);
    }
```

## Installation

```bash
    cordova plugin add https://github.com/AlexisCaffa/cordova-plugin-mediaac.git
```

## Supported Platforms

- Android (tested)

Without aac decoders:
- iOS
- Windows
- Browser

## Mediaac

```js
    var media = new Mediaac(src, mediaSuccess, [mediaError], [mediaStatus]);
```

### Parameters

- __src__: A URI containing the audio content. _(DOMString)_

- __mediaSuccess__: (Optional) The callback that executes after a `Mediaac` object has completed the current play, record, or stop action. _(Function)_

- __mediaError__: (Optional) The callback that executes if an error occurs. _(Function)_

- __mediaStatus__: (Optional) The callback that executes to indicate status changes. _(Function)_

__NOTE__: `cdvfile` path is supported as `src` parameter:
```javascript
var my_media = new Mediaac('cdvfile://localhost/temporary/recording.mp3', ...);
```

### Constants

The following constants are reported as the only parameter to the
`mediaStatus` callback:

- `Mediaac.MEDIA_NONE`     = 0;
- `Mediaac.MEDIA_STARTING` = 1;
- `Mediaac.MEDIA_RUNNING`  = 2;
- `Mediaac.MEDIA_PAUSED`   = 3;
- `Mediaac.MEDIA_STOPPED`  = 4;

### Methods

- `media.play`: Start or resume playing an audio file.

- `media.pause`: Pause playback of an audio file.

- `media.release`: Releases the underlying operating system's audio resources.

- `media.stop`: Stop playing an audio file.

## media.pause

Pauses playing an audio file.

    media.pause();


### Quick Example

```js
    // Play audio
    //
    function playAudio(url) {
        // Play the audio file at url
        var my_media = new Mediaac(url,
            // success callback
            function () { console.log("playAudio():Audio Success"); },
            // error callback
            function (err) { console.log("playAudio():Audio Error: " + err); }
        );

        // Play audio
        my_media.play();

        // Pause after 10 seconds
        setTimeout(function () {
            media.pause();
        }, 10000);
    }
```

## media.play

Starts or resumes playing an audio file.

```js
media.play();
```

### Quick Example

```js
    // Play audio
    //
    function playAudio(url) {
        // Play the audio file at url
        var my_media = new Mediaac(url,
            // success callback
            function () {
                console.log("playAudio():Audio Success");
            },
            // error callback
            function (err) {
                console.log("playAudio():Audio Error: " + err);
            }
        );
        // Play audio
        my_media.play();
    }
```

### iOS Quirks

- __numberOfLoops__: Pass this option to the `play` method to specify
  the number of times you want the media file to play, e.g.:

        var myMediaac = new Mediaac("http://audio.ibeat.org/content/p1rj1s/p1rj1s_-_rockGuitar.mp3")
        myMediaac.play({ numberOfLoops: 2 })

- __playAudioWhenScreenIsLocked__: Pass in this option to the `play`
  method to specify whether you want to allow playback when the screen
  is locked.  If set to `true` (the default value), the state of the
  hardware mute button is ignored, e.g.:

        var myMediaac = new Mediaac("http://audio.ibeat.org/content/p1rj1s/p1rj1s_-_rockGuitar.mp3")
        myMediaac.play({ playAudioWhenScreenIsLocked : false })
        myMedia.setVolume('1.0');

> Note: To allow playback with the screen locked or background audio you have to add `audio` to `UIBackgroundModes` in the `info.plist` file. See [Apple documentation](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html#//apple_ref/doc/uid/TP40007072-CH4-SW23). Also note that the audio has to be started before going to background.

- __order of file search__: When only a file name or simple path is
  provided, iOS searches in the `www` directory for the file, then in
  the application's `documents/tmp` directory:

        var myMediaac = new Mediaac("audio/beer.mp3")
        myMediaac.play()  // first looks for file in www/audio/beer.mp3 then in <application>/documents/tmp/audio/beer.mp3

## media.release

Releases the underlying operating system's audio resources.
This is particularly important for Android, since there are a finite amount of
OpenCore instances for media playback. Applications should call the `release`
function for any `Mediaac` resource that is no longer needed.

    media.release();


### Quick Example

```js
    // Audio player
    //
    var my_media = new Mediaac(src, onSuccess, onError);

    my_media.play();
    my_media.stop();
    my_media.release();
```

## media.stop

Stops playing an audio file.

    media.stop();

### Quick Example

```js
    // Play audio
    //
    function playAudio(url) {
        // Play the audio file at url
        var my_media = new Mediaac(url,
            // success callback
            function() {
                console.log("playAudio():Audio Success");
            },
            // error callback
            function(err) {
                console.log("playAudio():Audio Error: "+err);
            }
        );

        // Play audio
        my_media.play();

        // Pause after 10 seconds
        setTimeout(function() {
            my_media.stop();
        }, 10000);
    }
```

## MediaacError

A `MediaacError` object is returned to the `mediaError` callback
function when an error occurs.

### Properties

- __code__: One of the predefined error codes listed below.

- __message__: An error message describing the details of the error.

### Constants

- `MediaacError.MEDIA_ERR_ABORTED`        = 1
- `MediaacError.MEDIA_ERR_NETWORK`        = 2
- `MediaacError.MEDIA_ERR_DECODE`         = 3
- `MediaacError.MEDIA_ERR_NONE_SUPPORTED` = 4

