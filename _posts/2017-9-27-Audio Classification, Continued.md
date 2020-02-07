---
layout: post
title: Audio Classification, Continued

---

### [Get the code!](https://github.com/avikejriwal/Music-Genre-Identification)

Previously, I built a model to identify songs by genre.  Rather than predict a single genre for each song, I was able to predict a composition of genres.  However, the model was trained on 30-second audio samples, so the specific time window taken can have a considerable effect on the results when using the model on a full-length song.  

I was curious about how the predicted genre composition changes over time for a song.  I looked at using a moving window to display the genre composition of a song over time.  30-second sample windows were taken over the course of the entire song, with the window moving by a quarter of a second with each iteration.  The following video shows the genre distribution over time for one particular song (Despacito, by Luis Fonsi, Daddy Yankee ft. Justin Bieber).

<video src="/vid/Despacito.mp4" width="480" height="300" controls preload></video>

The distribution primarily switches between two dominant genres over time.  Otherwise, the changes are fairly small.
