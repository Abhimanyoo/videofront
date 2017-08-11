Transcoding module
==================

We are adding the `transcoding` to add the ability to transcode an extra
video format on top of the already existing format.


## Discovering transcoding formats


How Videfront knows about the transcoding format to use when a video
gets in for the first time ?


It uses this setting :


    ELASTIC_TRANSCODER_PRESETS = [
        ('LD', '1351620000001-000030', 900),  # System preset: Generic 480p 4:3
        ('SD', '1351620000001-000010', 2400), # System preset: Generic 720p
        ('HD', '1474271974931-39nker', 5400), # System preset: Generic 1080p - FUN (1080p + 1024x1024 thumbnails)
    ]
    

Now, imagine that you want to apply a new preset. You don't want to re-run the
3 initial transcoding operations again. That's why, we use use this module.

Here is the setting where you put the new format to be applied to existing videos.

    ELASTIC_TRANSCODER_NEW_PRESETS = [
        ('UL', '1499875722465-ygtqxq', 256),  # Fun-Mooc 320x240
    ]


## Usage example



This command looks at all videos for the given course and calculates the cost
for runnig the transcoding.


    transcode.estimate_cost('course-v1:fun+fun+session01')


This commnd runs the transcoding - this will also run the cost estimation
before transcoding.


    transcode.transcode_for_courses('course-v1:fun+fun+session01')


## Courses with multiple sessions



If you want to transcode videos for a course with multiple sessions, you will
be interested to read this.

Videofront puts videos in "Playlist". Each course is mapped to a playlist. That means, that
"Session 2" of a course has a different playlit than "Session 1".

Often, you will have videos spread within multiple playlists, beacuse there were multiple
runs of the course. How to make sure that you grab all videos for you mulltiple-session
course ?


You should pass all the sessions of the course this way - separated by spaces :

    transcode.transcode_for_courses('course-v1:fun+fun+session01 course-v1:fun+fun+session02 course-v1:fun+fun+session03')


## Logging



This module uses python logging to log in the console and also on file.

Look inside the code to see where log entries are sent. File logging would be for instance
in `/var/tmp/video-transcode.log`