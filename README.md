
# PiShrink #
PiShrink is a bash script that automatically shrink a pi image that will then resize to the max size of the SD card on boot. This will make putting the image back onto the SD card faster and the shrunk images will compress better.

## Usage ##
```
sudo pishrink.sh [-sdrzh] imagefile.img [newimagefile.img]
  -s: Skip autoexpand
  -d: Debug mode on
  -r: Use advanced repair options
  -z: Gzip compress image after shrinking
  -h: display help text
```

If you specify the `newimagefile.img` parameter, the script will make a copy of `imagefile.img` and work off that. You will need enough space to make a full copy of the image to use that option.

* `-s` will skip the autoexpanding part of the process.
* `-d` will create a logfile `pishrink.log` which may help for problem analysis.
* `-r` will attempt to repair the filesystem if regular repairs fail
* `-z` will Gzip compress the image after shrinking. The `.gz` extension will be added to the filename.


## Prerequisites ##
If you are trying to shrink a [NOOBS](https://github.com/raspberrypi/noobs) image it will likely fail. This is due to [NOOBS partitioning](https://github.com/raspberrypi/noobs/wiki/NOOBS-partitioning-explained) being significantly different than Raspbian's. Hopefully PiShrink will be able to support NOOBS in the near future.

If using Ubuntu, you will likely see an error about `e2fsck` being out of date and `metadata_csum`. The simplest fix for this is to use Ubuntu 16.10 and up, as it will save you a lot of hassle in the long run.

## Installation ##
```bash
wget https://raw.githubusercontent.com/Drewsif/PiShrink/master/pishrink.sh
chmod +x pishrink.sh
sudo mv pishrink.sh /usr/local/bin
```

## Example ##
```bash
[user@localhost PiShrink]$ sudo pishrink.sh pi.img
e2fsck 1.42.9 (28-Dec-2013)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/loop1: 88262/1929536 files (0.2% non-contiguous), 842728/7717632 blocks
resize2fs 1.42.9 (28-Dec-2013)
resize2fs 1.42.9 (28-Dec-2013)
Resizing the filesystem on /dev/loop1 to 773603 (4k) blocks.
Begin pass 2 (max = 100387)
Relocating blocks             XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Begin pass 3 (max = 236)
Scanning inode table          XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Begin pass 4 (max = 7348)
Updating inode references     XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
The filesystem on /dev/loop1 is now 773603 blocks long.

Shrunk pi.img from 30G to 3.1G
```

## macOS Support ##
It is not currently possible to run PiShrink natively in macOS. Included in the repo is a `Dockerfile` and `docker-compose.yml` that should allow you to run PiShrink on a Docker host, including a macOS one.

Note 1: The cloned repo is mounted as a Docker host mount so any output file should be created under `/pishrink` to have it persisted on the host.

Note 2: Only `*.img` is present in the provided `.dockerignore`, if you don't use this extension the created container will include your image, potentially consuming a significant amount of disk space. 

Example: `docker-compose run pishrink /pishrink/pishrink.sh /pishrink/someimage.img`

## Contributing ##
If you find a bug please create an issue for it. If you would like a new feature added, you can create an issue for it but I can't promise that I will get to it.

Pull requests for new features and bug fixes are more than welcome!
