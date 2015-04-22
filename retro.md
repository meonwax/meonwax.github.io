Vice
----
#### Start *crt* cartridge image
	x64 -cartcrt game.crt

#### Extract *d64* disk image
	c1541 game.d64 -extract

#### Extract *t64* container
	c1541 -format t,1 d64 /tmp/temp.d64 -tape game.t64 -extract && rm /tmp/temp.d64


FS-UAE
------

#### Start *adf* image
	fs-uae --floppy_drive_0=game.adf

Amiga DOS
---------
[AmigaDOS Command Examples](http://wiki.amigaos.net/wiki/AmigaOS_Manual:_AmigaDOS_Command_Examples)


Create an empty (unformatted) ADF image
---------------------------------------
    dd if=/dev/zero of=empty.adf count=1760


Minimum Bootable Workbench Disk
-------------------------------
	install df1:
	makedir df1:s
	makedir df1:c
	copy loadwb to df1:c/

Create a `s:startup-sequence` file, put

	Loadwb
	endcli

in it and save the file in `df1:s/`
