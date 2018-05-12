# Nightlies

(Currently only hosts Firefox Nightly, but has plans for more.)

Provides an Archlinux repo of a nightly binary without any manual updating or intervention needed.


## Why?

- Because I don't have the time.
- Because you don't want to wait.
- Because I wanted to see if it was technically possible.

### Why not?

- If you don't care about getting the latest nightly when it's out rather than several hours (or days!) later, use one of the AUR packages.
- If you don't trust me, either use an AUR package or run this software yourself.


## How to use it

Add this to your /etc/pacman.conf

```ini
[firefox-nightly]
Server = https://nightlies.passcod.name/firefox-nightly/$arch
```

Install the signing key:

```bash
# TODO
```

Install the package:

```bash
pacman -Sy firefox-nightly

# Alternatively, with auto-updates enabled:
pacman -Sy firefox-nightly-auto
# be sure to follow instructions
```


## How it works

1. When a request for the **repo database** comes in (i.e. when `pacman -Sy` is
   invoked), Nightlies checks its upstream (for Firefox Nightly, the Mozilla
   servers) for an update. If there is none, it happily returns the existing
   database file. If there is one, it quickly compiles a _new_ file with the
   new version number, and kicks off a generate process for the actual package.

2. When a request for the **package** comes in (i.e. when `pacman -S package`
   is invoked), Nightlies checks if the package version exists, and if it's
   not, whether there is a running generate process for it (or kicks one off).
   In the first case, it simply serves it. In the second, it blocks until the
   process finishes and then returns with the brand new package.

Nightlies generates uncompressed packages and databases _first_, so they're
available quickly, then generates _maximally compressed_ files so further
downloads are as efficient as possible.


## How to self host

1. Clone this repo
2. Make a signing key
3. Add the key ID to the config
4. Run `npm i`
5. Run `node .`
6. Point your pacman to the server

