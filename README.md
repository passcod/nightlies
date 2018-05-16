# Nightlies

Provides an Archlinux repo of a nightly binary without any manual updating or
intervention needed. Does its work lazily (no cron!) so won't incur any
bandwidth if it's not used.

(Currently only hosts Firefox Nightly, but has plans for more.)

## Why?

- Because I don't have the time.
- Because you don't want to wait.

### Why not?

- If you don't care about getting the latest nightly when it's out rather than
  several hours (or days!) later.
- If you don't trust me.
- If you're far from the server running this and the CDN isn't helping and you
  want better speeds.

Then either [use the AUR package][aur] or [run this software yourself][host].

[aur]: https://aur.archlinux.org/packages/firefox-nightly/
[host]: #how-to-self-host

## How to use it

Add this to your /etc/pacman.conf

```ini
[firefox-nightly]
Server = https://nightlies.passcod.name/firefox-nightly/$arch
SigLevel = PackageOptional
```

<!--
Install the signing key:

```bash
# TODO
```
-->

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
   database file. If there is one, it downloads and creates a new package as
   fast as it can, notably omitting any compression.

2. When that's done, a background process is kicked off to compress all files
   as much as it can and regenerate the repo metadata in consequence.


## How to self host

1. Clone this repo
<!--
2. Make a signing key
3. Add the key ID to the config
-->
4. Run `npm i`
5. Run `node .`
6. Point your pacman to the server

