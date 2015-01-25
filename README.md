# btfc2
complete rewrite of btfc
just shell script and transmission-cli

# How it works

INIT: It copies ~/btfc to ~/.btfccache/btfc, creates btfc.torrent, publishes the btfc.torrent (use gtfc to keep peers in sync), calls CLONE.

CLONE: It fetches the btfc.torrent (from gtfc), leeches/seeds to ~/.btfccache/btfc, once complete then it rsync skipping newer to ~/btfc then diff ~/.btfccache/btfc and ~/btfc to decide if it needs to call INIT.

# Quick Usage

First you need a working [gtfc cluster](https://github.com/johnko/gtfc).

You may want to enable Local Peer Discovery on all nodes (see Frequently Asked Questions below)

Then, on the source:

```
mkdir ~/btfc
date > ~/btfc/newfile
btfc -i
```

Then, on the peers:

```
mkdir ~/btfc
btfc -c
```

Wait for it to download.

# Speedup with hard links

If you are careful, you can use the "-l" option to speed up the INIT copy step with hard links. This options is UNSAFE if you will be appending or editing files in ~/btfc without first deleting them. This option is good for files that never change, and only if new files are added, but never modified.

# Properties

Property                                   | Description
-------------------------------------------|------------------
Centralized aspect                         | The first node with unique/latest data will be central until another completes a pull. Torrent trackers may be used.
Decentralized/distributed storage          | Kind of. When all nodes are up to date, there is no central store because completed torrents are mirrors. Akin to RAID1 mirror, but not to RAID5 span.
Decentralized/distributed transfers        | Yes.
Chunked data transfers from multiple hosts | Yes.
Optimal use                                | Any type of file.
Limitations                                | Can't use itself to sync itself, requires publish/fetch code to keep .torrent file in sync

# Frequently Asked Questions

### How do I restrict peers or keep my data private?

- use a good firewall
- only allow known/trusted peers to connect in to your BitTorrent port.
- only allow your nodes to connect out to known/trusted peers.
- don't expose your .torrent metadata.
- you may also want to run your own tracker.

### Does btfc use trackers?

Not by default.

It uses the [transmission-cli](https://www.transmissionbt.com/) which means it bootstraps with the DHT nodes provided by dht.transmissionbt.com.

To use a tracker, pass the "-t URL" (tracker URL). For multiple trackers use "-t URL" multiple times.

### How do I enable Local Peer Discovery?

In Transmission's configuration file (mine is located at `~/.config/transmission/settings.json`), find the line with "lpd-enabled" and set it to `"lpd-enabled": true,`

### How do I publish a .torrent so that all my peers will be able to subscribe to it?

My gtfc project is a good place to start: each peer will pull from each other using cron, git, SSH keys and fingerprints, excellent for small amounts of data like .torrent metadata.

Or something like WebDAV, or a network file system, or a clustered file system, but at that scale, why would you need this?
