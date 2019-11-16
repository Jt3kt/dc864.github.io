---
title: "Webcast and Recording Setup for Meetings"
date: 2019-11-16 12:00:00 +0000
layout: post
author: OverCast
twitter: "https://twitter.com/DefCon864"
permalink: blog/meeting-recording-setup.html
tags: [Meeting Webcast, Meeting Recording, Audio/Video]
---
# Webcast and Recording Setup for Meetings

## TL;DR:
- Webcast / web conference the meeting to remote attendees using Jitsi Meet
- Proxy intercept the presenter's HDMI feed
- OBS recording of the presenter and screen; edit with Kdenlive
- Audio / Video workflow with parts list
- Shared learning experience across the Upstate

## Overview

We have great meetings.  Instead of providing lecture or slide heavy presentations (nttawwt) our meetings are open for anyone to share what they are learning.  It's a learn-learn scenario for everyone envolved.  We celebrate the sharing of knowledge without regard to street cred or years of experience or the company you work for by day.  It would sure be nice to capture those lively discussions and knowledge sharing moments of goodness for later viewing pleasure or to share with those who can't make the in-person meeting.  Going forward DC864 will record and post to YouTube some of its meeting content.  We will also have a consistent web conference available for those remote souls unable to fight the traffic into downtown.

Why is this important and how do we see this playing out?  The Upstate is a large region and travel to the meeting isn't always possible.  We'd like anyone in the Upstate to be able to attend via the Webcast.  Those in college can pull together a meeting space on campus and host a webcast of the DC864 meeting.  In fact if you do host on campus we'd like you to consider yourselves members of the group as if you were here in-person.  Getting together for shared learning is the most imporant aspect.  For the main speaking time the presenter will, at their discression, be recorded for later upload to the DC864 YouTube channel.  So how are we going to make this happen?

## Diagram
![Setup Workflow](/images/overcast/recording-meetings/1911/meeting-recording-setup-diagram.png "the setup workflow")

## Parts list
Most of these items were repurposed hardware components.

Name | Cost | Comment
--- | ---: | ---
[Asus Vivobook 15 thin](https://www.amazon.com/ASUS-VivoBook-i3-8145U-Windows-F512FA-AB34/dp/B07RK5M35T) | $400.00 | The i3-8145U variety w/ 8GB RAM is a bare minimum spec
[Neewar NW-7000 USB Condenser Mic](https://www.amazon.com/Neewer-Condenser-Microphone-Professional-Broadcasting/dp/B07FMBMBHH) | $20.00 | Audio is king
[Mic stand](https://www.amazon.com/ammoon-Foldable-Microphone-Stand-Conferences/dp/B07HHTZVMV?th=1) | $12.00 | Future feature
[1080p webcam](https://www.amazon.com/Logitech-Widescreen-Calling-Recording-Desktop/dp/B006JH8T3S) | $50.00 | A member had a extra. 720p would work just as well.
[4-port USB Hub](https://www.amazon.com/gp/product/B00JX1ZS5O) | $8.00 | Cause laptops are limited on USB ports
[HDMI Capture: HDMI-in to USB and HDMI-out](https://www.amazon.com/gp/product/B07FXHN43Y) | $80.00 | Capture to USB while replicating HDMI-in to HDMI-out
[Portable monitor](https://www.amazon.com/Portable-Monitor-Computer-1920×1080-Protector/dp/B07RGPCQG1) | $170.00 | Dual screens is a must
[OBS Studio](https://obsproject.com/)|$0.00|Open Broadcaster Software
Optional: OBS Overlay & Theme| $0.00 | $10-30
[the Gimp](https://www.gimp.org/)|$0.00|The GNU Impage Manipulation Program
[Kdenlive](https://kdenlive.org/en/)||Libre Video Editor
YouTube account||Your soul and all first borns.
Optional: [Handbrake](https://handbrake.fr/)|$0.00|Open source video transencoder
__Total:__ | __$740.00__ | Cha-ching

But like I said, most tech-heads already have gear laying around.  Our out of pocket was only for the HDMI capture device.  The laptop is bare minimum specs and CPU pegged at 95+% for the entire recording session.  I don't edit on the laptop.  The laptop makes it so that I don't lug a PC dual displays and a keyboard to the meeting.

What about storage considerations?  The initial test run recording our November meeting was two hours in length.  When saved to MP4 the size is about 560 MB.  That's before tweaking and not 1080p so I expect this size will flux at the next meeting.  Not bad numbers to start.

## Things to Consider
Recording faces and voices is a privacy topic worth exploring.  Being up-front with the group before each and every meeting is important and we plan to have signage and verbal notices.  This is primarily why we don't stream directly to YouTube.  We are security professionals sharing life lessons from the trenches and a whole lot of funny stories.  Not all tales are appropriate for the world at large so we exercise creative editing license before any release.  We may also want to simply shorten the video and capture the core content.

## Setup
The recording laptop and components take up a decent bit of room on the table.  I may bring a foldable table for the AV gear if we start to outgrow the space.  The diagram above shows how each component connects to the laptop and logically connects to remote webcast participants.  Nothing too complicated about plugging cables, even I can handle that, or hosting a web conference, I can sometimes handle that.  Assume everything here will undergo significant changes in the months ahead as we play around with these options.

## Running the Meeting
1. The host launches OBS and verifies all the components are functioning with the correct sources in the overlay.
2. Mute all other microphone sources in Mixer leaving only the condenser mic hot.  
3. Right-click on the scene for the meeting and select Windowed Projector(Scene).  In a later step we'll share this window via Jitsi.  Move it to the second monitor and maximize.
4. Click the Start Recording button

**Note:** The most difficult aspect of this is the coordination of the meeting with remote participants.  In true Def Con fashion we hacked the initial recording session live working out the kinks as the meeting happened.  Failure is just another great learning opportunity.  

5. Start the Jitsi Meet session and share the URL in Slack.  Remote participants should be on mute until they have something to share.
6. Share the Windowed Projector(Scene) in Jitsi.

**Note:** The topic text in the OBS overlay is controlled by the contents of the DC864-Topic.txt file.  If we switch out speakers or flip between topics a simple edit of the text file is automatically picked up by OBS within a few seconds.
!(/images/overcast/recording-meetings/1911/topic-text.png "Topic text auto updates the OBS display")
7. Monitor the Slack and Jitsi chat during the meeting to troubleshoot or facilitate the remote discussion.
8. After the presentation click the Stop Recording button in OBS.

## Editing
9. Open Gimp and edit the intro and outro images to reflect the content.
10. Open Kdenlive and Add Clip to bring in the intro and outro images as well as the meeting video.
!(/images/overcast/recording-meetings/1911/kdenlive-add-clips.png "Add Clips")

11. Switch to Insert Mode to add the intro image, meeting video, and outro image to the timeline.
12. Switch to Normal Mode and select/drag to resize the intro image to four seconds in length.  Do the same for the outro image.
13. By default the timeline has two audio and video tracks.  Mute and disable video for V1 and A1.  I find it helpful while editing to move cut content into these tracks to ensure I've removed the correct segment.
14. When you find a segment to cut use the scissors tool and click the start and end of the track.  Move it into V1, the audio will automatically follow.
!(/images/overcast/recording-meetings/1911/kdenlive-cut1.png "Cutting out footage")

15. After all edits are complete and you are satisfied with the video playback click the Render button to produce the final video.

## Share
16. Video descriptions in YouTube are a key part of this process.  Take time to craft a solid description, work with the speaker as needed, and include links to any software or tools referenced.
17. Post the video to YouTube.
18. Post the description and video to the dc864.org website.
19. Tweet the new video and link to the site.  Those that want to can share on LinkedIn or other platforms.

So for now, that's it.  See you on December 5th in person or online.