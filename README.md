# btfc2
complete rewrite of btfc
just shell script and transmission-cli

# Potentially Asked Questions

### How do I restrict peers or keep my data private?
Use a good firewall and only allow known/trusted peers to connect to your BitTorrent port. Don't expose your .torrent metadata. You may also want to run your own tracker.

### Does btfc use trackers?
Yes. But you can configure it to use your own private trackers, -t multiple times.

### How do I publish a .torrent so that all my peers will be able to subscribe to it?
My gtfc project is a good place to start: each peer will pull from each other using cron, git, SSH keys and fingerprints, excellent for small amounts of data like .torrent metadata.
Or something like WebDAV, or a network file system, or a clustered file system, but at that scale, why would you need this?
