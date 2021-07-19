---
id: 20210719164818
uuid: 476ca90f-d6df-4107-84b9-b9f950377552
title: Borg Cheatsheet
date: 2021-07-19 16:48:18
modified: 
types: tech-note
categories: borg
pinned: false
tags: [borg, backup, deduplication, encryption, cheatsheet]
private: false
draft: false
---

### Initiation

```sh
# Create repository:
borg init --encryption repokey /mnt/backup/borg/my

# Export the repository key, as you need the key and passphrase for repository access (delete the key once you have backed it up):
borg key export /mnt/backup/borg/my/ /tmp/borg_my_key
```

### Automate Passphrase

```sh
# Create the passphrase file to store the passphrase (to allow for automation):
touch ~/.borg_my_passphrase

# Paste the passphrase into ~/.borg_my_passphrase

# Set permissions to user only:
chmod 400 ~/.borg_my_passphrase

# Then in .bashrc, your script, or crontab, point BORG_PASSCOMMAND to the passphrase file:
export BORG_PASSCOMMAND="cat $HOME/.borg_my_passphrase"
```

### Manage Backups

```sh
# Create archive (backup files):
borg create --verbose --progress --stats /mnt/backup/borg/my::$(date +%Y%m%d%H%M%S) /mnt/my/

# List archives in repository:
borg list /mnt/backup/borg/my/

# List contents of an archive:
borg list /mnt/backup/borg/my::<archive-name>

# Delete an archive:
borg delete /mnt/backup/borg/my::<archive-name>

```

```sh
borg prune $REPOSITORY --keep-daily=7 --keep-weekly=4 --keep-monthly=6
mkdir -p /tmp/borg
borg mount /data/myrepo::root-2013-08-02 /tmp/borg
borg umount /tmp/borg

# Check files in archive match the backed up files (backup integrity):
borg check --verify-data /mnt/backup/borg/my::<archive-name>

# Compare Contents of Archive to Local Filesystem:
borg export-tar /path/to/repo::archive-name - | tar --compare -f - -C /path/to/compare/to
```
