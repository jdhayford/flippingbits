# Popping the Hood on Video Streaming

## Outline
- Intro
  - Who this is for
  - Create desire to know
  - Examples:
    - Live:
      - Whether it be a Monday Night Football on ESPN
      - Silly example like live bird hatching


- Go to twitch
- Watching video
- Pop the hood, aka open up Chrome Developer tools (or your browser equivelant).


> Picture of Twitch with the developer panel open

Lets hop on over to the network tab to see whats going on

> Gif of the network traffic

Well that certainly looks active, if there's new live content coming in all the time it must be thrugh these requests.

Look at one of the requests.

Good lord its pretty ugly but heres what we do notice
- It s a bunch of GET requests to some files either ending in `.ts`

Joke about the response body, commenting that this is obviously a video based on the utf code pattern. Just kidding.

Lets see what this file is.
Ahh we notice that the file system recognizes this `.ts` file extensnion as MPEG-2 Transport Stream. Now if you aren't familary with MPEG, it stands for Moving Pictures Experts Group which is the working group that brought us neat things like the MP3 and MP4 (aside about MP3 pretty cool read How Music became Free). A quick wikipedia sesh of MPEG-2 revealse to use that it is `the generic coding of moving pictures and associated audio information`. Anyway looks like the system is giving us a few video player options, *shrug so lets give it a go.

> GIF of a 1-3 second video

Hey would you look at that! That looks like a piece of the video we're watching. Seems intuitive enough. So why are these segments so darn short? Well some of you smary pants might already have a gues or two but we'll get back to that later. All these pieces are getting grabbed as the stream continues. But how does it know where these crazy locations are at?

Brings us back to the `.m3u8` request. Lets take a peak in one of these. Hey would you look at that, its just a plain text file. THis is actually pretty reasonable. We can see some cool metadata all prefixed with `EXT`. Fun fact, the well known rapper DMX claims that this was what motivated his hit single "EXT gon' give it to you" (not true). The most interesting part of this file is the latter half where we see a list of a bunch of links. Actually those links look a whole lot like the one we tracked to get that video segment earlier. And the number matches here roughly to the duration of the video. Neato! A quick wikipedia gets us something called `M3U` which is described as such:

> (MP3 URL[1][2] or Moving Picture Experts Group Audio Layer 3 Uniform Resource Locator[3] in full) is a computer file format for a multimedia playlist.

Cool so these files act as playlists of the video segments for the stream. But how does this static file update? Notice that And the playlist seems like its the same location and it changes every time you get it. So at any point it includes the most recent N segments. And than all a web player would need to do is to continue to fetch this playlist and stich the newest seegments in.

Lets take a look at the very first playlist is. 

_Go into how this is a master playlist, which refers to others quality streams, which not only map to the quality options in the player but also talk about how these fancy players will look at how you're network is and can use these options to make sure you have a good playback experience aka can download content faster than you are watching it._

Lets look at some other ones
Super simple example from a beach webcam
Super fancy one that includes an encryption key

All of what we have seen so far using `.m3u8/.m3u` playlists and `.ts` video segments actually is following the HLS or HTTP Live Streaming protocol. Made by Apple and is one of the most used protocols for live streaming out there. Some other notable ones are DASH (Dynamic Adaptive Streaming over HTTP).

Cool so this has been a cool journey and know we have a bit of a better idea of our browsers pull in these video streams. Still wondering why some of these streams use such short segments? Well if you think about it, the shorter it is, the more often it can encode the original video, package it up and send it over to you. So really, the shorter these are, the less delay there is between you and "live". So why not make these even shorter. Well this trends into even more interesting topic of Low Latency streaming. But some quick notes are that
****
a) The shorter the segments, the more cuts, the more work a streaming architetcture has to do
b) You still cant get over the real latency between you and the source
c) There are also some interesting size implications. What does that mean? Well videos are able to compress their size so well by describing changes in the video over time rather than use every frame which would be huge (looking at you GIF). So it becomes very quickly diminishing returns game.

ITS MY STREAM AND I WANT IT NOW

Use glass to glass, neat buzz. Mention where it does matter, i.e. for sports or other applications where trimming a few seconsd or even half a second matter to people.
