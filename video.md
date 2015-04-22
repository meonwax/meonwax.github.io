Save a DVB-T stream in MPEG2-TS format using VLC
------------------------------------------------
    vlc dvb-t://frequency=738000000:inversion=0:bandwidth=8:code-rate-hp=2/3:code-rate-lp=1/2:modulation=16QAM:transmission=-1:guard=1/4:hierarchy=0 --sout file/ts:/path/to/target.ts


Download aus ARD-Mediathek
--------------------------
	http://www.ardmediathek.de/play/media/[DOCUMENT_ID]?devicetype=pc&features=flash


Download aus ZDF-Mediathek
--------------------------
	http://www.zdf.de/ZDFmediathek/xmlservice/web/beitragsDetails?id=[DOCUMENT_ID]&ak=web


MEncoder Xvid (2-pass)
----------------------
	mencoder source.mp4 -ovc xvid -xvidencopts pass=1 -vf scale=720:576 -oac mp3lame -o /dev/null
	mencoder source.mp4 -ovc xvid -xvidencopts pass=2:bitrate=1500 -vf scale=720:576 -oac mp3lame -lameopts cbr:mode=3:br=128 -o target.avi
[Gentoo Wiki](http://www.gentoo-wiki.info/MEncoder/Rip_DVD#Xvid)


FFmpeg Xvid
-----------
Convert to XVid using VBR with quality 8 and 192 kbit/s MP3 and rescale it

	ffmpeg -i source.mp4 -s 640x480 -vcodec libxvid -qscale 8 -acodec libmp3lame -ab 192000 target.avi
	
[Source](http://nothings.org/remote/ffmpeg.txt)


Convert videos to be Android/iOS compatible
-------------------------------------------
To make a video playable on every Android/iOS device with its builtin video player, use *ffmpeg* to convert it to a compatible format.  
Use 2-pass-encoding to achieve an effective compression using ABR (Average Bit Rate)

	ffmpeg -y -i input.avi -c:v libx264 -b:v 1500k -pass 1 -an -profile:v baseline -level 3.0 -pix_fmt yuv420p -f mp4 /dev/null
	ffmpeg -i input.avi -c:v libx264 -b:v 1500k -pass 2 -c:a aac -b:a 128k -strict -2 -profile:v baseline -level 3.0 -pix_fmt yuv420p target.mp4


Videos on HTML5 sites
---------------------
To be sure *all* major browsers are able to display the video content, convert the video source into these three formats.  
Details [here](http://www.html5rocks.com/de/tutorials/video/basics/).  

#### MP4
    ffmpeg -i source.mp4 -vcodec libx264 -vf scale=480:-1 -profile:v baseline -level 3 -strict -2 target.mp4

#### OGG THEORA
    ffmpeg2theora source.mp4 --optimize --audioquality 4 --videoquality 5 -x 480 -o target.ogv

#### WEBM
    ffmpeg -i source.mp4 -c:v libvpx -crf 10 -b:v 1M -vf scale=480:-1 -c:a libvorbis target.webm

#### Including HTML
	<video>
	  <source src="target.webm" type='video/webm; codecs="vp8, vorbis"' />
	  <source src="target.mp4" type='video/mp4; codecs="avc1.42E01E, mp4a.40.2"' />
	  <source src="target.ogv" type='video/ogg; codecs="theora, vorbis"' />
	  Video tag not supported. Download the video <a href="target.webm">here</a>.
	<video>


DVD rippen und zu XVid konvertieren
-----------------------------------
	dvdbackup -M -i /dev/sr0 -o [DVD_DIRECTORY]
	lsdvd [DVD_DIRECTORY]
	for i in 3 4 5 6; do mplayer dvd://$i/[DVD_DIRECTORY]/ -dumpstream -dumpfile title$i.vob; done
	for i in 3 4 5 6; do ffmpeg -i title$i.vob -filter:v yadif -vcodec libxvid -q:v 5 -s 640x480 -acodec libmp3lame -ab 128000 title$i.avi; done


Spiel im Fenstermodus aufnehmen
------------------------------
1. Ausführen und Spielefenster anklicken

		xwininfo

2. Erhaltene Werte hier einsetzen:

		ffmpeg -f pulse -ac 2 -i default -f x11grab -r 30 -s WIDTHxHEIGHT -i :0.0+XPOSITION,YPOSITION -acodec pcm_s16le -vcodec libx264 -preset ultrafast -crf 0 -threads 0 record.mkv

3. Final komprimieren, z.B. mit  

		ffmpeg -i record.mkv -acodec libmp3lame -ab 128k -ac 2 -vcodec libx264 -vpre slow -crf 22 -threads 0 output.mp4
