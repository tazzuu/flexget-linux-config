# Flexget config for automated RSS feed downloads of .torrent files

So that you can use Deluge torrent client with automated RSS feed without needing the [YaRSS2 plugin](https://dev.deluge-torrent.org/wiki/Plugins/YaRSS2) that does not work well with seedboxes or headless torrent client management.

Because c'mon, who are we kidding, the Deluge plugin installation and management interface is [incredibly stupid](https://dev.deluge-torrent.org/wiki/Plugins#InstallingPlugins). Its 2024, we dont download Python egg files matched for some arbitrary Python version (who even has a "system Python" anymore??).

[This](https://www.rapidseedbox.com/kb/use-rss-deluge-using-yarss2-plugin) is how we are supposed to be enabling RSS support for Deluge on our remote servers? Uh, thanks Diego, but noooo thanks. Not a fan of [other methods](https://www.reddit.com/r/seedboxes/comments/yf0qn4/using_rss_with_deluge_on_a_seedbox/) either.

-----

So you want to configure your Deluge torrent client on your headless server (or seedbox)? Instead of glueing your RSS retrieval to a specific client, the ["best"](https://www.reddit.com/r/torrents/comments/amriu4/comment/efo4i7n/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1) method is to just offload the RSS handling to a separate service.

This demo config will be using [Flexget](https://github.com/Flexget/Flexget) in order to automate the retrieval of .torrent files from an RSS feed. This super simple example will deposit the files into directory. Then, you can use the built-in "AutoAdd" Deluge plugin (no install necessary) to automatically import the new .torrent files into your Deluge service.

## RSS Feeds

We will be using these two websites to get an RSS feed of free open Linux ISO torrents to download automatically

- Distrowatch: https://distrowatch.com/news/torrents.xml
- LinuxTracker: https://linuxtracker.org/index.php?page=modules&module=getrss
  - you might need to make an account and log in first before this page works
  - once logged in, come back to this page and "Check All" then select "Feed Type: Download Link" and click the "Get RSS" button for your personal RSS URL
  - or use the [anonymous Ubuntu feed](https://askubuntu.com/questions/24229/is-there-an-rss-feed-of-ubuntu-release-torrent-files); https://linuxtracker.org/rss_torrents.php?feed=dl&cat[]=563&pid=00000000000000000000000000000000

## Usage

### Setup

First, make sure you have Docker and `docker compose` installed.

You can run the Docker container interactively with this command to make sure its working

```bash
docker compose up flexget
```

If it works, you should be able to log in from your web browser at `http://localhost:5050/login` using the password included in the `docker-compose.yml` file.

If everything is good then you should Ctrl-C in your terminal to stop running Docker in the foreground, and use this command to launch Flexget as a background container service

```bash
docker compose up -d
```

If you need to see what is happening in the container in the background, you use this command

```bash
docker logs flexget
```

### Config

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