List a processes that are listening on network ports
----------------------------------------------------
	lsof -i -n -P | grep -e LISTEN
	netstat -tunlp


Execute a command for every file in directory
---------------------------------------------
	for f in *.ape; do ffmpeg -i "$f" "${f%%.*}.wav"; done


Search for files recursively and execute a command
--------------------------------------------------
	find . -name "*.txt" -type f -exec program


Rename all files from upper- to lowercase in the current directory
------------------------------------------------------------------
This will only work with filenames containing no spaces:

	for f in * ; do mv -v $f `echo $f | tr '[A-Z]' '[a-z]'`; done

This will only work with filenames containing spaces:

	find . -depth -name '* *' | while IFS= read -r f ; do mv -v "$f" "$(dirname "$f")/$(basename "$f"|tr '[A-Z]' '[a-z]')" ; done

You have to use both if you have a mix of filenames with spaces and without them in your folder.
[Source](https://www.garron.me/en/bits/rename-files-to-lower-case-linux-mac.html)


Set FAT label
-------------
	mlabel -i /dev/sdh1 ::"MYLABEL"
	
	
Create USB stick with UDF filesystem
------------------------------------
	dd if=/dev/zero of=/dev/sdh bs=1M count=1
	mkudffs -b 512 --media-type=hd /dev/sdh
	
[Source](http://tanguy.ortolo.eu/blog/article93/usb-udf)

Copy a local public key onto a server for SSH access
----------------------------------------------------
	ssh-keygen -t rsa
	ssh-copy-id -i ~/.ssh/id_rsa.pub user@server


Expose any local server behind a NAT or firewall to the Internet using SSH tunnel
---------------------------------------------------------------------------------
	ssh -vN user@yourserver.com -R [remoteport]:localhost:[localport]


Use SSH tunnel to access web GUI of a router in a home network
--------------------------------------------------
	ssh -vN -L 8080:192.168.1.1:80 user@homeip.com

Then open `http://localhost:8080` in a webbrowser


Create a SOCKS Proxy through an SSH Tunnel
---------------------------------------
	ssh -vN -D 1080 user@yourserver.com
	
Configure your webbrowser to use a proxy and set `127.0.0.1` as your SOCKS host at port `1080`.


Show external ip address
------------------------
	dig +short myip.opendns.com @resolver1.opendns.com
	curl checkip.spdns.de


RubyGems
--------
#### Remove outdated gems that are installed
	gem outdated

#### Update all the installed gems
	gem update
	
#### Remove outdated versions of gems that are installed
	gem clean
	
#### Get rid of a gem completely
	gem uninstall mysql

#### List of locally installed gems
	gem list
	
#### List gems along with access times
	gem stale
	

Create a copy of local Git repository on a remote server
--------------------------------------------------------

1. Create a new bare repository on the server

		git init --bare repo.git

2. Add it as origin in local repo and initially push

		git remote add origin ssh://user@server/var/git/repo.git
		git push --set-upstream origin master
	

Vim: Replace in whole document
------------------------------
	:%s/foo/bar/g


Vimdiff
-------

`Ctrl-W + Ctrl-W` Switch to the other split window.  
`do` Get changes from other window into the current window.  
`dp` Put the changes from current window into the other window.  
`]c` Jump to the next change.  
`[c` Jump to the previous change.


Compile kernel modules for linux kernel >= 3.8
----------------------------------------------

Remove all references to `__devinit`, `__devexit` and `__devexit_p` from the module sources as these have been removed in 3.8

	sed -i 's/__devinit//g; s/__devexit_p//g; s/__devexit//' *.c


Sync a directory to another
---------------------------
	nice rsync -rlptDogx -P -v --delete /source/ /destination/

[Full system backup with rsync](https://wiki.archlinux.org/index.php/full_system_backup_with_rsync)


Encrypt files with password using gpg and upload it
---------------------------------------------------
	cat hello.txt | gpg -ac -o- | curl -X PUT --upload-file "-" https://transfer.sh/hello.txt.gpg
	
Decrypt it again

	wget -O - https://transfer.sh/lcqHc/hello.txt.gpg | gpg -d -o hello.txt


Tomcat deployment using cURL on the commandline
-----------------------------------------------
	curl -T package.war https://user:password@example.com:8080/manager/text/deploy?update=1&path=/ROOT


DNS Server
----------
`156.154.70.1`				DNS Advantage  
`193.151.4.201`				ispOne  
`213.73.91.35`				dnscache.berlin.ccc.de  
`8.8.8.8` und `8.8.4.4`		Google Public DNS
