# Skate Motion Capture

A browser-based application for **automatically detecting and recording skateboard movements**.

The application uses the camera of a smartphone or tablet to continuously analyze the camera image for movement. When sufficient movement is detected, video recording starts automatically. Once the movement has stopped, the recording continues for a configurable cooldown period and is then automatically downloaded as a video clip.

No backend or server infrastructure is required.

## Live Demo

**Try Skate Motion Capture directly in your browser:**

[https://fabshaw.github.io/skateboard-tool/](https://fabshaw.github.io/skateboard-tool/?utm_source=chatgpt.com)

> Camera and microphone access are required. For security reasons, browsers generally require the application to be served over HTTPS.

## Features

* 📷 Camera access directly from the browser
* 🎥 Automatic video recording when movement is detected
* 🔍 Real-time image-based motion detection
* ⚙️ Adjustable motion detection sensitivity
* 📊 Live display of the detected motion level
* ⏱️ Configurable recording cooldown time
* 🎞️ Selectable video frame rate: 24, 30 or 60 FPS
* 👀 Optional live camera preview
* 📱 Designed for use on mobile devices
* 🔒 Screen Wake Lock support to keep the display active during use
* 💾 Settings stored locally using `localStorage`
* 📁 Automatic download of detected skate clips

## How It Works

The motion detection runs entirely **client-side in the browser**.

For motion detection, the camera image is scaled down to a resolution of **160 × 120 pixels**. This significantly reduces the amount of data that needs to be processed and helps reduce CPU and battery usage on mobile devices.

The current frame is then compared with the previous frame. For every pixel, the difference between the red, green and blue color channels is calculated.

If the combined RGB difference exceeds the configured sensitivity value, the pixel is considered to have changed.

The application calculates the percentage of changed pixels and uses this value as the current **motion ratio**.

If the motion ratio exceeds the configured motion threshold, video recording starts automatically.

When the motion ratio remains below the threshold for the configured cooldown period, recording stops and the resulting clip is downloaded automatically.

## Settings

The application provides the following settings:

| Setting          |            Range |   Default |
| ---------------- | ---------------: | --------: |
| Sensitivity      |            0–255 |        50 |
| Motion Threshold |         0.5–10 % |     1.5 % |
| Cooldown         |     1–10 seconds | 4 seconds |
| Video FPS        | 24 / 30 / 60 FPS |    30 FPS |

The settings are stored in the browser's `localStorage` and are automatically restored when the application is opened again.

### Sensitivity

Controls how large the RGB difference between two pixels has to be before a pixel is considered to have changed.

* **Lower value:** more sensitive to movement
* **Higher value:** less sensitive to movement

### Motion Threshold

Defines how much of the camera image has to change before movement is detected.

For example:

```text
Motion Threshold = 1.5 %
```

If more than 1.5% of the analyzed pixels change sufficiently, the application considers this to be movement.

### Cooldown

Defines how long recording continues after the last detected movement.

For example:

```text
Cooldown = 4 seconds
```

This prevents the video from ending immediately when a skateboard trick or movement finishes.

### Video FPS

The recording frame rate can be set to:

* 24 FPS
* 30 FPS
* 60 FPS

Higher frame rates can be useful for analyzing fast skateboard tricks, but generally require more processing power and storage.

## Camera

The application requests access to the device camera and microphone.

The **rear-facing camera** is preferred on mobile devices using:

```javascript
facingMode: "environment"
```

The requested camera resolution is up to 1280 × 720 pixels, while the lower-resolution image is used internally for motion detection.

> The browser must grant permission to access the camera and microphone.

## Video Recording

Video recording uses the browser's native **MediaRecorder API**.

The application first attempts to use:

```text
video/webm;codecs=vp9,opus
```

If this codec is not supported, it falls back to:

```text
video/webm;codecs=vp8,opus
```

and finally attempts:

```text
video/mp4
```

The resulting video is automatically downloaded when recording stops.

File names contain a timestamp, for example:

```text
Skate_Clip_2026-09-15T06-45-32-123Z.webm
```

## Typical Workflow

### 1. Open the application

Open the application in a modern browser:

[Skate Motion Capture – Live Demo](https://fabshaw.github.io/skateboard-tool/?utm_source=chatgpt.com)

### 2. Enable the live preview

Click **"Enable Live Preview"** and grant permission to access the camera and microphone.

The rear camera is preferred when available.

### 3. Start motion detection

Click **"Start Detection"**.

The application now waits for movement:

```text
Waiting for movement...
```

### 4. Perform a skateboard trick

As soon as sufficient movement is detected, recording starts automatically:

```text
● RECORDING...
```

### 5. Finish the trick

When movement stops, the recording continues for the configured cooldown period.

### 6. The clip is downloaded automatically

Once recording has stopped, the resulting video clip is automatically downloaded by the browser.

## Tips for Better Motion Detection

For reliable motion detection, position the camera so that:

* the skateboarder is clearly visible,
* the camera remains stationary,
* the skateboarder contrasts well with the background,
* the recording area contains as little unnecessary movement as possible,
* the entire trick can be seen within the camera frame.

A relatively static background generally produces more reliable results because background movement can otherwise trigger the motion detector.

## Wake Lock

The application uses the browser's **Screen Wake Lock API**, when supported.

This can prevent the device screen from automatically turning off while the application is being used.

If the page becomes visible again after being hidden, the application attempts to re-acquire the Wake Lock.

> Wake Lock support depends on the browser and device.

## Privacy

The application is designed to perform its processing directly in the browser.

No backend server is required for motion detection or video recording.

The camera stream is processed locally by the browser, and the generated video clips are downloaded directly to the user's device.

Application settings are stored locally using the browser's `localStorage`.

## Technical Overview

The project is implemented using standard browser technologies:

* HTML5
* CSS3
* JavaScript
* MediaDevices API
* MediaRecorder API
* Canvas API
* LocalStorage API
* Screen Wake Lock API

No external JavaScript frameworks or libraries are required.

## Project Structure

The application can be deployed as a single HTML file:

```text
.
└── index.html
```

HTML, CSS and JavaScript are currently contained in this single file.

## Browser Requirements

The browser needs to support the following APIs:

* `navigator.mediaDevices.getUserMedia()`
* `MediaRecorder`
* `<video>`
* `<canvas>`
* `localStorage`
* optionally, the Screen Wake Lock API

A modern Chromium-based browser is recommended, especially when using the application on Android devices.

## Deployment

Since the application is entirely client-side, it can be hosted on any static web server.

Examples include:

* GitHub Pages
* Nginx
* Apache
* any other static hosting provider

No Node.js server, database or backend service is required.

## License

If this project is intended to be published as open source, add an appropriate open-source license to the repository, such as the MIT License.

---

**Skate Motion Capture**

Automatic motion detection and video recording for skateboard tricks — directly in the browser.
