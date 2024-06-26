---
title: Media Capture With MP4 Video Capture for Android / iOS
description: Capture audio, video, and images. Modified for use with Alpha Anywhere
---

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

# cordova-plugin-media-capture-mp4video

This plugin provides access to the device's audio, image, and video capture capabilities.

This plugin has been modified to return an mpeg encoded mp4 file when video is captured from from Android or iOS device.

**WARNING**: Collection and use of images, video, or
audio from the device's camera or microphone raises important privacy
issues. Your app's privacy policy should discuss how the app uses
such sensors and whether the data recorded is shared with any other
parties. In addition, if the app's use of the camera or microphone is
not apparent in the user interface, you should provide a just-in-time
notice before the app accesses the camera or microphone (if the
device operating system doesn't do so already). That notice should
provide the same information noted above, as well as obtaining the
user's permission (e.g., by presenting choices for **OK** and **No
Thanks**). Note that some app marketplaces may require your app to
provide just-in-time notice and obtain permission from the user prior
to accessing the camera or microphone. For more information, please
see the Privacy Guide.

This plugin defines global `navigator.device.capture` object.

Although in the global scope, it is not available until after the `deviceready` event.

    document.addEventListener("deviceready", onDeviceReady, false);
    function onDeviceReady() {
        console.log(navigator.device.capture);
    }

## Installation

     cordova plugin add https://github.com/remoorejr/cordova-plugin-media-capture-mp4video.git


## Supported Platforms

- Android
- iOS

## Objects

- Capture
- CaptureAudioOptions
- CaptureImageOptions
- CaptureVideoOptions
- CaptureCallback
- CaptureErrorCB
- ConfigurationData
- MediaFile
- MediaFileData

## Methods

- capture.captureAudio
- capture.captureImage
- capture.captureVideo
- MediaFile.getFormatData

## Properties

- **supportedAudioModes**: The audio recording formats supported by the device. (ConfigurationData[])

- **supportedImageModes**: The recording image sizes and formats supported by the device. (ConfigurationData[])

- **supportedVideoModes**: The recording video resolutions and formats supported by the device. (ConfigurationData[])

## capture.captureAudio

> Start the audio recorder application and return information about captured audio clip files.

    navigator.device.capture.captureAudio(
        CaptureCB captureSuccess, CaptureErrorCB captureError,  [CaptureAudioOptions options]
    );

### Description

Starts an asynchronous operation to capture audio recordings using the
device's default audio recording application. The operation allows
the device user to capture multiple recordings in a single session.

The capture operation ends when either the user exits the audio
recording application, or the maximum number of recordings specified
by `CaptureAudioOptions.limit` is reached. If no `limit` parameter
value is specified, it defaults to one (1), and the capture operation
terminates after the user records a single audio clip.

When the capture operation finishes, the `CaptureCallback` executes
with an array of `MediaFile` objects describing each captured audio
clip file. If the user terminates the operation before an audio clip
is captured, the `CaptureErrorCallback` executes with a `CaptureError`
object, featuring the `CaptureError.CAPTURE_NO_MEDIA_FILES` error
code.

### Supported Platforms

- Android
- iOS

### Example

    // capture callback
    var captureSuccess = function(mediaFiles) {
        var i, path, len;
        for (i = 0, len = mediaFiles.length; i < len; i += 1) {
            path = mediaFiles[i].fullPath;
            // do something interesting with the file
        }
    };

    // capture error callback
    var captureError = function(error) {
        navigator.notification.alert('Error code: ' + error.code, null, 'Capture Error');
    };

    // start audio capture
    navigator.device.capture.captureAudio(captureSuccess, captureError, {limit:2});

### iOS Quirks

Since iOS 10 it's mandatory to add a `NSCameraUsageDescription`, `NSMicrophoneUsageDescription` and `NSPhotoLibraryUsageDescription` in the info.plist.

- `NSCameraUsageDescription` describes the reason that the app accesses the user’s camera.
- `NSMicrophoneUsageDescription` describes the reason that the app accesses the user’s microphone.
- `NSPhotoLibraryUsageDescription` describes the reason the app accesses the user's photo library.

When the system prompts the user to allow access, this string is displayed as part of the dialog box.

These strings have been hard coded with the following strings to allow the plugin to be used by PhoneGap Build as well as the Cordova CLI:

- `NSCameraUsageDescription:` This app requires access to the camera to take photos.
- `NSMicrophoneUsageDescription:` This app requires access to the microphone to record audio.
- `NSPhotoLibraryUsageDescription:` This app requires access to the photo library to display images.

*

* iOS does not have a default audio recording application, so a simple user interface is provided.

### Description

Starts an asynchronous operation to capture images using the device's
camera application. The operation allows users to capture more than
one image in a single session.

The capture operation ends either when the user closes the camera
application, or the maximum number of recordings specified by
`CaptureImageOptions.limit` is reached. If no `limit` value is
specified, it defaults to one (1), and the capture operation
terminates after the user captures a single image.

When the capture operation finishes, it invokes the `CaptureCB`
callback with an array of `MediaFile` objects describing each captured
image file. If the user terminates the operation before capturing an
image, the `CaptureErrorCB` callback executes with a `CaptureError`
object featuring a `CaptureError.CAPTURE_NO_MEDIA_FILES` error code.

### Supported Platforms

- Android
- iOS

### Example

    // capture callback
    var captureSuccess = function(mediaFiles) {
        var i, path, len;
        for (i = 0, len = mediaFiles.length; i < len; i += 1) {
            path = mediaFiles[i].fullPath;
            // do something interesting with the file
        }
    };

    // capture error callback
    var captureError = function(error) {
        navigator.notification.alert('Error code: ' + error.code, null, 'Capture Error');
    };

    // start image capture
    navigator.device.capture.captureImage(captureSuccess, captureError, {limit:2});

## capture.captureVideo

> Start the video recorder application and return information about captured video clip files.

    navigator.device.capture.captureVideo(
        CaptureCB captureSuccess, CaptureErrorCB captureError, [CaptureVideoOptions options]
    );

### Description

Starts an asynchronous operation to capture video recordings using the
device's video recording application. The operation allows the user
to capture more than one recordings in a single session.

The capture operation ends when either the user exits the video
recording application, or the maximum number of recordings specified
by `CaptureVideoOptions.limit` is reached. If no `limit` parameter
value is specified, it defaults to one (1), and the capture operation
terminates after the user records a single video clip.

When the capture operation finishes, it the `CaptureCB` callback
executes with an array of `MediaFile` objects describing each captured
video clip file. If the user terminates the operation before
capturing a video clip, the `CaptureErrorCB` callback executes with a
`CaptureError` object featuring a
`CaptureError.CAPTURE_NO_MEDIA_FILES` error code.

### Supported Platforms

- Android
- iOS

### Example

    // capture callback
    var captureSuccess = function(mediaFiles) {
        var i, path, len;
        for (i = 0, len = mediaFiles.length; i < len; i += 1) {
            path = mediaFiles[i].fullPath;
            // do something interesting with the file
        }
    };

    // capture error callback
    var captureError = function(error) {
        navigator.notification.alert('Error code: ' + error.code, null, 'Capture Error');
    };

    // start video capture
    navigator.device.capture.captureVideo(captureSuccess, captureError, {limit:2});

## CaptureAudioOptions

> Encapsulates audio capture configuration options.

### Properties

- **limit**: The maximum number of audio clips the device user can record in a single capture operation. The value must be greater than or equal to 1 (defaults to 1).

- **duration**: The maximum duration of an audio sound clip, in seconds.

### Example

    // limit capture operation to 3 media files, no longer than 10 seconds each
    var options = { limit: 3, duration: 10 };

    navigator.device.capture.captureAudio(captureSuccess, captureError, options);

### Android Quirks

- The `duration` parameter is not supported. Recording lengths can't be limited programmatically.

### iOS Quirks

- The `limit` parameter is not supported, so only one recording can be created for each invocation.

## CaptureImageOptions

> Encapsulates image capture configuration options.

### Properties

- **limit**: The maximum number of images the user can capture in a single capture operation. The value must be greater than or equal to 1 (defaults to 1).

### Example

    // limit capture operation to 3 images
    var options = { limit: 3 };

    navigator.device.capture.captureImage(captureSuccess, captureError, options);

### iOS Quirks

- The **limit** parameter is not supported, and only one image is taken per invocation.

## CaptureVideoOptions

> Encapsulates video capture configuration options.

### Properties

- **limit**: The maximum number of video clips the device's user can capture in a single capture operation. The value must be greater than or equal to 1 (defaults to 1).

- **duration**: The maximum duration of a video clip, in seconds.

### Example

    // limit capture operation to 3 video clips
    var options = { limit: 3 };

    navigator.device.capture.captureVideo(captureSuccess, captureError, options);

### iOS Quirks

- The **limit** property is ignored. Only one video is recorded per invocation.

### Android Quirks

- Android supports an additional **quality** property, to allow capturing video at different qualities. A value of `1` ( the default ) means high quality and value of `0` means low quality, suitable for MMS messages.
  See [here](http://developer.android.com/reference/android/provider/MediaStore.html#EXTRA_VIDEO_QUALITY) for more details.

### Example ( Android w/ quality )

    // limit capture operation to 1 video clip of low quality
    var options = { limit: 1, quality: 0 };
    navigator.device.capture.captureVideo(captureSuccess, captureError, options);

## CaptureCB

> Invoked upon a successful media capture operation.

    function captureSuccess( MediaFile[] mediaFiles ) { ... };

### Description

This function executes after a successful capture operation completes.
At this point a media file has been captured, and either the user has
exited the media capture application, or the capture limit has been
reached.

Each `MediaFile` object describes a captured media file.

### Example

    // capture callback
    function captureSuccess(mediaFiles) {
        var i, path, len;
        for (i = 0, len = mediaFiles.length; i < len; i += 1) {
            path = mediaFiles[i].fullPath;
            // do something interesting with the file
        }
    };

## CaptureError

> Encapsulates the error code resulting from a failed media capture operation.

### Properties

- **code**: One of the pre-defined error codes listed below.

### Constants

- `CaptureError.CAPTURE_INTERNAL_ERR`: The camera or microphone failed to capture image or sound.

- `CaptureError.CAPTURE_APPLICATION_BUSY`: The camera or audio capture application is currently serving another capture request.

- `CaptureError.CAPTURE_INVALID_ARGUMENT`: Invalid use of the API (e.g., the value of `limit` is less than one).

- `CaptureError.CAPTURE_NO_MEDIA_FILES`: The user exits the camera or audio capture application before capturing anything.

- `CaptureError.CAPTURE_PERMISSION_DENIED`: The user denied a permission required to perform the given capture request.

- `CaptureError.CAPTURE_NOT_SUPPORTED`: The requested capture operation is not supported.

## CaptureErrorCB

> Invoked if an error occurs during a media capture operation.

    function captureError( CaptureError error ) { ... };

### Description

This function executes if an error occurs when trying to launch a
media capture operation. Failure scenarios include when the capture
application is busy, a capture operation is already taking place, or
the user cancels the operation before any media files are captured.

This function executes with a `CaptureError` object containing an
appropriate error `code`.

### Example

    // capture error callback
    var captureError = function(error) {
        navigator.notification.alert('Error code: ' + error.code, null, 'Capture Error');
    };

## ConfigurationData

> Encapsulates a set of media capture parameters that a device supports.

### Description

Describes media capture modes supported by the device. The
configuration data includes the MIME type, and capture dimensions for
video or image capture.

The MIME types should adhere to [RFC2046](http://www.ietf.org/rfc/rfc2046.txt). Examples:

- `video/3gpp`
- `video/mpeg`
- `image/jpeg`
- `audio/amr`
- `audio/wav`

### Properties

- **type**: The ASCII-encoded lowercase string representing the media type. (DOMString)

- **height**: The height of the image or video in pixels. The value is zero for sound clips. (Number)

- **width**: The width of the image or video in pixels. The value is zero for sound clips. (Number)

### Example

    // retrieve supported image modes
    var imageModes = navigator.device.capture.supportedImageModes;

    // Select mode that has the highest horizontal resolution
    var width = 0;
    var selectedmode;
    for each (var mode in imageModes) {
        if (mode.width > width) {
            width = mode.width;
            selectedmode = mode;
        }
    }

Not supported by any platform. All configuration data arrays are empty.

## MediaFile.getFormatData

> Retrieves format information about the media capture file.

    mediaFile.getFormatData(
        MediaFileDataSuccessCB successCallback,
        [MediaFileDataErrorCB errorCallback]
    );

### Description

This function asynchronously attempts to retrieve the format
information for the media file. If successful, it invokes the
`MediaFileDataSuccessCB` callback with a `MediaFileData` object. If
the attempt fails, this function invokes the `MediaFileDataErrorCB`
callback.

### Supported Platforms

- Android
- iOS

### Android Quirks

The API to access media file format information is limited, so not all
`MediaFileData` properties are supported.

### iOS Quirks

The API to access media file format information is limited, so not all
`MediaFileData` properties are supported.

## MediaFile

> Encapsulates properties of a media capture file.

### Properties

- **name**: The name of the file, without path information. (DOMString)

- **fullPath**: The full path of the file, including the name. (DOMString)

- **type**: The file's mime type (DOMString)

- **lastModifiedDate**: The date and time when the file was last modified. (Date)

- **size**: The size of the file, in bytes. (Number)

### Methods

- **MediaFile.getFormatData**: Retrieves the format information of the media file.

## MediaFileData

> Encapsulates format information about a media file.

### Properties

- **codecs**: The actual format of the audio and video content. (DOMString)

- **bitrate**: The average bitrate of the content. The value is zero for images. (Number)

- **height**: The height of the image or video in pixels. The value is zero for audio clips. (Number)

- **width**: The width of the image or video in pixels. The value is zero for audio clips. (Number)

- **duration**: The length of the video or sound clip in seconds. The value is zero for images. (Number)

### Android Quirks

Supports the following `MediaFileData` properties:

- **codecs**: Not supported, and returns `null`.

- **bitrate**: Not supported, and returns zero.

- **height**: Supported: image and video files only.

- **width**: Supported: image and video files only.

- **duration**: Supported: audio and video files only.

### iOS Quirks

Supports the following `MediaFileData` properties:

- **codecs**: Not supported, and returns `null`.

- **bitrate**: Supported on iOS4 devices for audio only. Returns zero for images and videos.

- **height**: Supported: image and video files only.

- **width**: Supported: image and video files only.

- **duration**: Supported: audio and video files only.

## Android Lifecycle Quirks

When capturing audio, video, or images on the Android platform, there is a chance that the
application will get destroyed after the Cordova Webview is pushed to the background by
the native capture application. See the [Android Lifecycle Guide][android-lifecycle] for
a full description of the issue. In this case, the success and failure callbacks passed
to the capture method will not be fired and instead the results of the call will be
delivered via a document event that fires after the Cordova [resume event][resume-event].

In your app, you should subscribe to the two possible events like so:

```javascript
function onDeviceReady() {
  // pendingcaptureresult is fired if the capture call is successful
  document.addEventListener("pendingcaptureresult", function (mediaFiles) {
    // Do something with result
  });

  // pendingcaptureerror is fired if the capture call is unsuccessful
  document.addEventListener("pendingcaptureerror", function (error) {
    // Handle error case
  });
}

// Only subscribe to events after deviceready fires
document.addEventListener("deviceready", onDeviceReady);
```

It is up you to track what part of your code these results are coming from. Be sure to
save and restore your app's state as part of the [pause][pause-event] and
[resume][resume-event] events as appropriate. Please note that these events will only
fire on the Android platform and only when the Webview was destroyed during a capture
operation.

[android-lifecycle]: http://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html#lifecycle-guide
[pause-event]: http://cordova.apache.org/docs/en/latest/cordova/events/events.html#pause
[resume-event]: http://cordova.apache.org/docs/en/latest/cordova/events/events.html#resume

## iOS Privacy Manifest

As of May 1, 2024, Apple requires a privacy manifest file to be created for apps and third-party SDKs. The purpose of the privacy manifest file is to explain the data being collected and the reasons for the required APIs it uses. Starting with `cordova-ios@7.1.0`, APIs are available for configuring the privacy manifest file from `config.xml`.

This plugin comes pre-bundled with a `PrivacyInfo.xcprivacy` file that contains the list of APIs it uses and the reasons for using them.

However, as an app developer, it will be your responsibility to identify additional information explaining what your app does with that data.

In this case, you will need to review the "[Describing data use in privacy manifests](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_data_use_in_privacy_manifests)" to understand the list of known `NSPrivacyCollectedDataTypes` and `NSPrivacyCollectedDataTypePurposes`.

Also, ensure all four keys—`NSPrivacyTracking`, `NSPrivacyTrackingDomains`, `NSPrivacyAccessedAPITypes`, and `NSPrivacyCollectedDataTypes`—are defined, even if you are not making an addition to the other items. Apple requires all to be defined.
