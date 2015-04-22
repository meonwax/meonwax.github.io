Alle Prozesse anzeigen, die auf Netzwerk-Ports lauschen
-------------------------------------------------------
	lsof -i -n -P | grep -e LISTEN
	netstat -tunlp


Für jede Datei im Verzeichnis Befehl ausführen
----------------------------------------------
	for f in *.ape; do ffmpeg -i "$f" "${f%%.*}.wav"; done

Dateien suchen und Befehl ausführen
-----------------------------------
	find . -name "*.txt" -type f -exec program


FAT-Label setzen
----------------
	mlabel -i /dev/sdh1 ::"MYLABEL"
	
	
Create USB stick with UDF filesystem
------------------------------------
	dd if=/dev/zero of=/dev/sdh bs=1M count=1
	mkudffs -b 512 --media-type=hd /dev/sdh
	
[Source](http://tanguy.ortolo.eu/blog/article93/usb-udf)

Public key für SSH Zugang auf Server laden
------------------------------------------
	ssh-keygen -t rsa
	ssh-copy-id -i ~/.ssh/id_rsa.pub user@server


Use SSH tunnel to access web GUI of a router in a home network
----------------------------------------------------------
	ssh -vN -L 8080:192.168.1.1:80 user@homeip -p 2223

Then open `http://localhost:8080` in a webbrowser


Create a SOCKS Proxy through an SSH Tunnel
------------------------------------------
	ssh -vN -D 1080 user@server -p 2223
	
Configure your webbrowser to use a proxy and set `127.0.0.1` as your SOCKS host at port `1080`.


Externe IP-Adresse anzeigen
---------------------------
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
	

Vim: Ersetzen im gesamten Dokument
----------------------------------
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


DNS Server
----------
`156.154.70.1`				DNS Advantage  
`193.151.4.201`				ispOne  
`213.73.91.35`				dnscache.berlin.ccc.de  
`8.8.8.8` und `8.8.4.4`		Google Public DNS