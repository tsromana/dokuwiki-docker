# DokuWiki docker container

This is a fork of mparsil/dokuwiki, where this docker image lets you mount volumes directly from the host rather then through docker containers (though this should continue to work too).

## To run image:

For a quick run, build the image then as run as

    docker run -d -p 80:80 --name my_wiki {{ your tag }}

You can now visit the install page to configure your new DokuWiki wiki.

For example, if you are running container locally, you can acces the page 
in browser by going to http://127.0.0.1/install.php

### Using data containers:

To save data using volume containers, create a new container with

    docker run --volumes-from my_wiki --name my_wiki_data busybox true

To start a container with the above volumes, use the command

    docker run -d -p 80:80 --name my_wiki --volumes-from my_wiki_data deadleg/dokuwiki

### Using volumes from host:

You will need the add the following volumes from the host

* dokuwiki/data
* dokuwiki/lib/plugins
* dokuwiki/lib/tpl
* dokuwiki/conf

E.g. add the commands

    -v /host/dokuwiki/data:/dokuwiki/data -v /host/dokuwiki/lib/plugins:/dokuwiki/lib/plugins -v /host/dokuwiki/lib/tpl:/dokuwiki/lib/tpl -v /host/dokuwiki/conf:/dokuwiki/conf

The host folders _must_ be writable by the `www-data` user (UID 33, GID 33).

If you are mounting these folders from a samba share, you can add some combination of the following to your mounting options:

    uid=33,gid=33,forceuid,forcegid,noperm,file_mode=0777,dir_mode=0777
    
(These are probably not all needed, setting the uid and gid might be sufficient).

# Optimizing your wiki

Lighttpd configuration also includes rewrites, so you can enable 
nice URLs in settings (Advanced -> Nice URLs, set to ".htaccess")

For better performance enable xsendfile in settings.
Set to proprietary lighttpd header (for lighttpd < 1.5)

# Build your own

	docker build -t my_dokuwiki .
