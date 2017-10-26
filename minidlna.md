`minidlna`

```
shell> brew install minidlna
```

```
shell> mkdir -p ~/.config/minidlna
shell> cp /usr/local/opt/minidlna/share/minidlna/minidlna.conf ~/.config/minidlna/minidlna.conf
shell> ln -s YOUR_MEDIA_DIR ~/.config/minidlna/media
shell> minidlnad -f ~/.config/minidlna/minidlna.conf -P ~/.config/minidlna/minidlna.pid
```

minidlna.conf
```
friendly_name=Mac DLNA Server
media_dir=/Users/user/.config/minidlna/media
db_dir=/Users/user/.config/minidlna/cache
log_dir=/Users/user/.config/minidlna
```

```
shell> brew services start minidlna
shell> minidlna
```

- http://manpages.ubuntu.com/manpages/xenial/man5/minidlna.conf.5.html
