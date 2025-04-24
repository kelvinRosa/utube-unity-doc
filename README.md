# YoutubeSimplifiedRequest Documentation

## Overview
`YoutubeSimplifiedRequest` is a Unity script that simplifies YouTube video playback directly in Unity, supporting multiple video qualities, formats, and features like 360° and 3D videos.

## Dependencies
- Unity 2019.4 or higher
- Unity Video module installed
- Newtonsoft.Json (for JSON parsing)
- SimpleJSON (for JSON parsing)

## Public Variables

### Basic Configuration
- `youtubeUrl`: URL of the YouTube video to play
- `videoQuality`: Video quality (Standard, Hd, Fullhd, Uhd1440, Uhd2160)
- `is360`: Whether the video is 360 degrees
- `videoFormat`: Video format (Mp4 or Webm)

### Playback Options
- `autoPlayOnStart`: Automatically plays on start
- `startFromSecond`: Starts playback from a specific time
- `showThumbnailBeforeVideoLoad`: Shows thumbnail before video loads
- `customPlaylist`: Enables custom playlist
- `autoPlayNextVideo`: Automatically plays next video in playlist

### Advanced Settings
- `playUsingInternalDevicePlayer`: Uses native mobile device video player
- `loadYoutubeUrlsOnly`: Only loads URLs without playing
- `is3DLayoutVideo`: Enables 3D mode
- `layout3d`: 3D layout type (SideBySide, OverUnder, etc.)
- `mainCamera`: Main camera for rendering
- `videoPlayer`: VideoPlayer component for video
- `audioPlayer`: VideoPlayer component for audio (HD+ qualities)

## Public Methods

### Playback Control
- `PlayYoutubeVideo(string videoUrl)`: Starts video playback
- `Play()`: Resumes paused playback
- `Pause()`: Pauses playback
- `Stop()`: Stops playback completely

### Navigation
- `SkipToPercent(float pct)`: Skips to specific percentage of video
- `TrySkip(PointerEventData eventData)`: Attempts to skip based on UI event

## Events
The script triggers several events through the `YoutubeVideoEvents` component:
- `OnVideoReadyToStart`: When video is ready to start
- `OnVideoStarted`: When playback begins
- `OnVideoPaused`: When video is paused
- `OnVideoFinished`: When video ends
- `OnYoutubeUrlAreReady`: When URLs are ready (loadYoutubeUrlsOnly mode)
- `YoutubeTimedEvent`: Array of events that trigger events at specific video time and optionally pause the video waiting for an action from the user.

### VideoTimeEvents
The system allows adding time-based events during video playback. You can configure specific actions to trigger when the video reaches predefined timestamps without additional coding. These events are triggered when the current playback time reaches the preset values.

## Internal Workflow

### Main Flow
1. Extracts video ID from provided URL
2. Makes request to YouTube API to get stream URLs
3. Processes response and extracts video/audio URLs
4. Configures VideoPlayers with obtained URLs
5. Manages synchronization between video and audio (for HD+ qualities)

### Quality Handling
- **Standard (360p)**: Uses single VideoPlayer with integrated audio
- **Higher qualities**: Uses two VideoPlayers (one for video, one for audio) with manual synchronization

## Important Notes
- For 360° videos, the script automatically adjusts the skybox
- 3D videos require specially configured materials
- On mobile devices, may use native player for better performance
- Audio/video synchronization in high qualities is experimental

## Limitations
- Qualities above 720p may have synchronization issues
- Some restricted videos may not work
- Requires internet connection to obtain stream URLs

## Basic Usage Example
- You simply can use our prefab with the YoutubePlayer script or you can create custom functionalities if you want using the requester script (YoutubeSimplifiedRequest[You need the setup the callbacks])

```csharp
// Basic Inspector setup:
// - Set youtubeUrl
// - Configure desired videoQuality
// - Assign videoPlayer and audioPlayer (if HD+)

// To play programmatically:
GetComponent<YoutubeSimplifiedRequest>().PlayYoutubeVideo("https://youtu.be/VIDEO_ID");
