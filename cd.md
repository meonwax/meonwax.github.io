Burn ISO image
--------------
	wodim -dao -v dev=/dev/sr0
	

Burn CD/DVD image with cuesheet/TOC
-----------------------------------
	cdrdao write -n --driver generic-mmc-raw --device /dev/sr0 file.toc
	
	
Blank CD-RW
-----------
	wodim -v dev=/dev/sr0 -blank=fast


Raw CD Rip
----------
	cdrdao read-cd --device /dev/sr0 --driver generic-mmc-raw --read-raw --datafile output.bin output.toc

Copy audio CD DAO with one CD drive
-----------------------------------
    cdrdao copy --device /dev/sr0 --driver generic-mmc-raw --read-raw toc-file.toc 

This will prompt you to insert the CD-R after an image of the source CD was created. The image file with name `cddata<pid>.bin` will be created in the current working directory and removed when done.

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
