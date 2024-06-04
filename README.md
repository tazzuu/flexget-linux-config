# FlexGet config for automated RSS feed downloads of .torrent files

So that you can use Deluge torrent client with automated RSS feed without needing the [YaRSS2 plugin](https://dev.deluge-torrent.org/wiki/Plugins/YaRSS2) that does not work well with seedboxes or headless torrent client management.

Because c'mon, who are we kidding, the Deluge plugin installation and management interface is [incredibly stupid](https://dev.deluge-torrent.org/wiki/Plugins#InstallingPlugins). Its 2024, we dont download Python egg files matched for some arbitrary Python version (who even has a "system Python" anymore??).

[This](https://www.rapidseedbox.com/kb/use-rss-deluge-using-yarss2-plugin) is how we are supposed to be enabling RSS support for Deluge on our remote servers? Uh, thanks Diego, but noooo thanks. Not a fan of [other methods](https://www.reddit.com/r/seedboxes/comments/yf0qn4/using_rss_with_deluge_on_a_seedbox/) either.

So you want to configure your Deluge torrent client on your headless server (or seedbox)? Instead of glueing your RSS retrieval to a specific client, the ["best"](https://www.reddit.com/r/torrents/comments/amriu4/comment/efo4i7n/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1) method is to just offload the RSS handling to a separate service.

# https://distrowatch.com/news/torrents.xml
# https://linuxtracker.org/index.php?page=modules&module=getrss

# commands;
```bash
flexget execute --tasks linuxtracker-rss --no-cache
flexget execute --tasks linuxtracker-rss --now
flexget --test execute --tasks linuxtracker-rss
```

# Resources

- FlexGet web pages
  - https://github.com/Flexget/Flexget
  - https://flexget.com/Plugins/rss
  -  https://flexget.com/en/Configuration

- container taken from here: https://docs.linuxserver.io/images/docker-flexget/