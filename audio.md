Compress audio in movies
------------------------

Extract audio

	ffmpeg -i video.avi -ac 2 -y audio_clean.wav

Extract video
	
	ffmpeg -i video.avi -map 0:0 -vcodec copy -y video_clean.avi

Compress audio
	
	sox --show-progress audio_clean.wav -t wav audio_compressed.wav compand 0.0,1 6:-70,-43,-20 -6 -90 0.1

Combine audio and video
	
	ffmpeg -i video_clean.avi -i audio_compressed.wav -vcodec copy video_compressed.avi

Remove temporary files

	rm video_clean.avi
	rm audio_clean.wav
	rm audio_compressed.wav

Some example [values](http://forum.doom9.org/showthread.php?t=165807) for the `sox compand` command.

Optionally audio could be normalized (maximized volume without clipping) using `sox`.
