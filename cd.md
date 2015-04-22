ISO Image brennen
-----------------
	wodim -dao -v dev=/dev/sr0
	

CD/DVD Image mit Cuesheet/TOC brennen
---------------------------------
	cdrdao write -n --driver generic-mmc-raw --device /dev/sr0 file.toc
	
	
CD-RW löschen
-------------
	wodim -v dev=/dev/sr0 -blank=fast


Raw CD Rip
----------
	cdrdao read-cd --device /dev/sr0 --read-raw --datafile output.bin output.toc


Create an Audio-CD from one large MP3, split into single tracks
---------------------------------------------------------------
Convert the MP3 file to WAVE with `ffmpeg -i source.mp3 -ar 44100 -ac 2 target.wav`

Assume you have a WAVE file `live.wav` that contains several minutes of live music and you want to divide it into tracks of 5 minutes each with further subdivisions by index marks.  
Create the following toc-file `live.toc`:

    CD_DA
    TRACK AUDIO
    FILE "live.wav" 0 5:0:0

    TRACK AUDIO
    FILE "live.wav" 5:0:0 5:0:0

    TRACK AUDIO
    FILE "live.wav" 10:0:0 5:0:0
    ...
    
Then use `cdrdao write --device /dev/sr0 live.toc` to create a CD without any pre-gaps between the tracks.


Create an Audio-CD with a secret pre-gap before 1st track
---------------------------------------------------------
The toc-file

    CD_DA
    TRACK AUDIO
    FILE "secret-pregap.wav"
    
    START
    FILE "track1.wav"
    
    TRACK AUDIO
    FILE "track2.wav"
    ...
    
will create a CD with a pre-gap before track 1 that contains audio data from the file `secret-pregap.wav`.  
You will have to scan backwards when playing the first track with your CD player to hear the "secret" music.


Rip a complete Audio-CD into a single V0 MP3 file (for DJ sets or live albums)
---------------------------------------------------------------------
	cdparanoia 1- - | lame -V 0 - output.mp3
