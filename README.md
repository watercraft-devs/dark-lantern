# Dark Lantern Encrypted DNS Toggle

**Toggle encrypted DNS with NO dependency on systemd**

Dark Lantern routes all your DNS lookups through an `Unbound resolver` running at localhost.

The resolver forwards the lookups to `Quad9` over `DNS-over-TLS`. 

( Quad 9 default in this release, future releases will provide other options. )

This makes the lookups not visible or alterable by your network and ISP.

Toggle it on and off with `enable` and `disable`, 

Many public networks block encrypted DNS so toggle it off when needed.

NO dependency on `systemd`, init system agnostic.

It uses `Unbound` and `NetworkManager` only.





## Usage

    sudo ./dark-lantern-dns.sh enable
    sudo ./dark-lantern-dns.sh disable
    sudo ./dark-lantern-dns.sh status

`enable` tests a real lookup after switching and reverts automatically if it
fails.


## Functionality

- creates `/etc/unbound/unbound.conf.d/watercraft-dot.conf` and `/etc/NetworkManager/conf.d/90-watercraft-dns.conf` 
- points `/etc/resolv.conf/` at localhost `127.0.0.1`
- makes a backup of the original at  `/etc/resolv.conf.pre-watercraft`
- restore to the backup and cleanup all created files with the `disable` command


## Things to know

- Some networks block port 853, we currently don't set a fallback that is triggered by this case so if you must use a network like this you have to disable with `sudo ./dark-lantern-dns.sh disable`.

- Hostnames that only the local network's DNS knows about wont resolve because the query goes to the DNS provider (Quad9 in this version). 
    - `.local` names via `mDNS` should still work 

- Captive portals such as those in hotel or airport wifi may not work at all or require you to visit the `http` version of the URL before the `https` version will work.
    - If it doesn't work at all you have to disable with `sudo ./dark-lantern-dns.sh disable`.

- This version defaults to Quad9, we like them as an encrypted DNS provider for now but we will create a path towards modular optionality of providers in future releases.


