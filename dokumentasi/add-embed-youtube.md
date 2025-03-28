# Add Embed Youtube In Markdown

### Step 1: Get the YouTube Video ID

The YouTube video ID is the part of the URL after the v=. For example, in the URL

```
[https://www.youtube.com/watch?v=Xl1oZQBH7xA](https://www.youtube.com/watch?v=Xl1oZQBH7xA)
```

the video ID is **Xl1oZQBH7xA**. You'll need that for the next step

### Step 2: Embed the Video

To embed the video, use the following markdown

```
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg)](https://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE)

```

YouTube already has a thumbnail image for the video, so you can use that. Just replace YOUTUBE_VIDEO_ID_HERE with the video ID you got in step 1 in the first URL (for the image). Then again in the second URL (for the link to the video).
