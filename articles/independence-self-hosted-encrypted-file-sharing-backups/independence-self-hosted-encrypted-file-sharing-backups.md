# Independence: Self-hosted, encrypted file-sharing & backups

Dropbox, Google Drive, MEGA or any other "cloud" storage has one big issue. There is a trusted third party in the setup. They can read, and manipulate your data, or restrict access to them. In this article I present the way how I have cracked this problem.

## Requirements
1. Synchronize files between devices and have them "in the cloud" to keep always online copy
2. Automated backups
3. Decentralized, no trusted party
4. Keep files encrypted on all devices and "in the cloud"
5. Communication between devices must be encrypted and hidden
6. Interoperability between devices (server, desktop, phone)

## Solution
Synchronization and encryption are two different problems and should be solved independently. It is important is to use an encryption tool, that encrypts files separately, so the synchronization can be done per file.

1. Use [gocryptfs](https://nuetzlich.net/gocryptfs/) or [Cryptomator](https://cryptomator.org/) to encrypt files on the devices.
2. Use [Syncthing](https://syncthing.net/) to synchronize files between devices.
3. Backup data on the server (one node in Syncthing) by cron-job

### Encryption
First we create the encrypted `vault` directory locally. This directory will hold all our precious files. We use an encryption software to protect this directory.

#### gocryptfs
- Installing gocryptfs is easy, because it is available in packages by default (`apt install gocryptfs`).
- There is [unofficial Android app](https://github.com/hardcore-sushi/DroidFS) that will be hopefully [soon on F-Droid](https://github.com/hardcore-sushi/DroidFS/issues/8) as well.
- It is easily manageable from CLI. The UI is only via [third party app SiriKali](https://mhogomchungu.github.io/sirikali/).

~I prefer this one over Cryptomator because of simple CLI usage and free Android app.~ I am currently using Cryptomator because of [this](https://github.com/rfjakob/gocryptfs/issues/549) issue.

#### Cryptomator
- [Downloading](https://cryptomator.org/downloads/) and installing is easy. There is also [DEB repository](https://launchpad.net/~sebastian-stenzel/+archive/ubuntu/cryptomator).
- [Android app](https://cryptomator.org/android/) is paid (one time payment, lifetime license). It also seems it will be [available on F-Droid soon](https://github.com/cryptomator/android/issues/1#issuecomment-752992114).

### Synchronization
Now we need to set up synchronization to replicate this encrypted directory across all our devices.

#### Syncthing
Syncthing is a great concept, it synchronizes data between multiple devices without central server (Cloud). Every time two devices are online at the same time, the Syncthing will propagate changes between them.

My setup is: Notebook, Android phone and the server. I have added the server to the setup for two reasons: 1) It is always online, and 2) it allows me to run backup scripts there regularly.

1. [Installation](https://syncthing.net/downloads/) is easy and straight forward. I recommend to use Debian/Ubuntu repository to keep Syncthing updated.
2. The Next step is [Starting Syncthing Automatically](https://docs.syncthing.net/users/autostart.html) after every reboot. On Linux I recommend using [`systemd`](https://docs.syncthing.net/users/autostart.html#using-systemd)
3. Syncthing should [use Tor](https://docs.syncthing.net/users/proxying.html).
   In case `systemd` it means add into `/etc/systemd/system/syncthing@user.service` following likes:
   ```
   Environment="all_proxy=socks5://127.0.0.1:9050"
   Environment="ALL_PROXY_NO_FALLBACK=1"
   ```
4. Syncthing runs at `http://127.0.0.1:8384` by default. Add it to bookmarks.

#### Syncthing on Android
- Syncthing [app is available on F-Droid](https://f-droid.org/en/packages/com.nutomic.syncthingandroid/)
- Make sure to turn Tor on in settings (requires [Orbot](https://guardianproject.info/apps/org.torproject.android/))


#### Backup
If you accidentally delete a file, the Syncthing propagates the deletion. To prevent data loss, we need backups. Follow [this article](https://linuxconfig.org/keep-your-home-safe-with-cron-backups) to set up cronjob for regular backups. (The [crontab.guru](https://crontab.guru/) site is great help for figuring out the cron settings.)

#### Syncthing on Server
The server is needed:
- to have one node that is always online to propagate changes between devices that are not online at the same time,
- and for backups.

There is a good [article](https://theselfhostingblog.com/posts/how-to-set-up-a-headless-syncthing-network/) that describes how to configure such a server. It is important to **set the username and password** after the GUI is exposed publicly on the server.

1. Create user `syncthing`: `sudo adduser syncthing --shell=/bin/false`
2. Add Synthing repository and `sudo apt install syncthing`
3. Setup [autostart using `systemd`](https://docs.syncthing.net/users/autostart.html#using-systemd)
4. Allow access for GUI from outside in `config.xml`, change `127.0.0.1:8384` to `0.0.0.0:8384`
5. Visit GUI and **set the username and password**
6. Setup sync directory
7. Create `/home/syncthing/backup.sh` file containing:
   ```
   #! /bin/bash
   tar --listed-incremental=/media/backup/snapshot.file -cJpf /media/backup/the-vault-gocryptfs-backup-`date +%Y-%m-%d-%H-%M`.tar.xz /home/syncthing/<sync_dir_name>
   ```
8. Create file `/home/syncthing/restore.sh` containing:
    ```
    #! /bin/bash

    ls /media/backup | grep tar.xz | sort | xargs -I'{}' tar --extract --verbose --verbose --listed-incremental=/dev/null --file=/media/backup/{}
    ```   
9. Add executable flag `chmod +X /home/user/backup.sh`, `chmod +X /home/user/restore.sh`   
19. Setup backup directory `sudo mkdir /media/backup`, `sudo chown syncthing /media/backup -R`, `sudo chgrp syncthing /media/backup -R`
11. Add cronjob by `sudo crontab -e -u syncthing` and add line
    ```
    0 0 * * * /home/syncthing/backup.sh
    ```
12. Test everything by adding files and changing the cron timer for next minute to validate the backups are created


#### Testing backup
***Without testing a backup, you do not have any backup at all.***

1. We stop `syncthing` by `sudo systemctl stop syncthing@syncthing.service` to prevent propagation of changes
2. We simulate the disaster by running `sudo rm -fr /home/syncthing/<sync_dir_name>`
3. By `cd /media/backup` we switch directory recover data by: `sudo /home/syncthing/restore.sh`
5. We move `sudo mv /media/backup/home/syncthing/<sync_dir_name> /home/syncthing/`
6. Verify all files are in place (espetially `gocryptfs.conf` and `gocryptfs.diriv` in the root directory)
7. Start syncthing `sudo systemctl start syncthing@syncthing.service`
8. Verify the only changes propagated through Syncthing are the files' "Modified time" metadata

### Sources
- [Reddit: End-to-End encrypted self-hosted centralized storage](https://www.reddit.com/r/selfhosted/comments/cddewx/endtoend_encrypted_selfhosted_centralized_storage/)
- [How to Set Up a Headless Syncthing Network](https://theselfhostingblog.com/posts/how-to-set-up-a-headless-syncthing-network/)
- [Keep Your /home Safe With Cron Backups](https://linuxconfig.org/keep-your-home-safe-with-cron-backups)
- [Incremental Backups with Tar](https://www.theurbanpenguin.com/incremental-backups-with-tar/)

