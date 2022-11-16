# Get it

## Clients

The first and oldest clients is written in Python and is simply called ["Magic Wormhole"](https://github.com/magic-wormhole/magic-wormhole). It has been packaged for a lot of Linux distros and might easily available through your package manager. It also runs on Windows, although no convenient packaging is available. It is the only one supporting Tor connections.

["wormhole-rs"](https://github.com/magic-wormhole/magic-wormhole.rs) is the Rust rewrite of the Python client. It has provides a few new or improved features (most notably better direct connections and port forwarding), while not having reached feature parity in other aspects. You are unlikely to find it in your package manager, but there are dependencyless Linux and Windows (both x86-64) binaries that you can download.

["Wormhole William"](https://github.com/psanford/wormhole-william) implements the protocols in Go. It shines by providing statically compiled binaries for a *lot* of different platforms and computer architectures. This means that even though it may not be packaged for your system, there are really high chances that you may be able to run it. Among the platforms are Windows 32 bit and ARMv6 (e.g. Raspberry Pi 1).

All those clients are command line interfaces. There are GUI clients too:

["Warp"](https://gitlab.gnome.org/felinira/warp) is a Gtk based client primarily targeting Linux systems.

["Rymdport"](https://github.com/Jacalz/rymdport) is a cross platform GUI using the Fyne UI toolkit.

## Libraries and bindings?



## Applications using Magic Wormhole

["tmux-wormhole"](https://github.com/gcla/tmux-wormhole) provides interesting features (e.g. transparently open a remote file locally) based on Magic Wormhole.

["termshark"](https://github.com/gcla/termshark) has a built-in `wormhole` command that copies files over from a remote device.

## Alternatives

Magic Wormhole is not the first software with this approach, nor will it be the last one.

- [pcp](https://github.com/dennis-tra/pcp): Wormhole-like application built on libp2p (the library powering [IPFS](https://ipfs.io/)). It is fully peer-to-peer and thus requires no explicit rendezvous-server.
- [croc](https://schollz.com/blog/croc6/): Inspired by Magic Wormhole, with a few new features
- Shall we mention wormhole DOT app here? :D
- [teleport](https://gitlab.gnome.org/jsparber/teleport) provides file transfer within a local network. It has automatic device discovery (so no codes) and excellent system integration.
- If you want to minimize transferred data across similar directories, use [rsync](https://en.wikipedia.org/wiki/Rsync) instead.
- If you want regular file synchronization across devices, try [syncthing](https://syncthing.net/).
- Mozilla's abandoned [Firefox Send](https://en.wikipedia.org/wiki/Firefox_Send) shall get an honorable mention, as one of the projects inspiring Magic Wormhole.
