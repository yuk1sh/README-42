The domain name is the part of your internet address to the right of your host name.
It is often something that ends in .com .net, .edu, or .org.
If you are setting up a home network, you can make something up, but make sure you use the same domain name on all your computers.

[!!] set up users and password

A user account will be created for you to use instead of the root account for non-administrative activities.

Please enter the real name of this user.
This infomation will be used for instance as default origin for emails sent by this user as well as any program which displays or uses the user’s real name.
Your full name is a reasonable choice.

[!!] set up users and password

Select a username for the new account, Your first name is reasonable choice.
The username should start with a lower-case letter, which can be followed by any combination of numbers and more lower-case letters.

















[!!] Partition disks （Partitonを設定の7番め手順）

Summary of current LVM configuration:

Free Physical Volumes: 0
Used Physical Volumes: 1
Volume Groups:         1
Logical Volumes:       2

LVM configuration action:
Display configuration details
Create volume group
Create logical volume
Delete logical volume
Extend volume group
Finish

[!!] Partition disks （Partitonを設定の9番め手順直後の選択肢）
Display configuration details
Create volume group
Finish

[!!] Partition disks （Partitonを設定の12番めで#5を選択した時のポップアップ）

No modifications can be made to the partition #5 device SCSI1 (0,0,0) (sda) for the following reasons:

In use by LVM volume group user42-vg

<Continue>

[!!] Partition disks （Partitonを設定の15番の<Yes>を選択した時のポップアップ）
The data on SCSI1 (0,0,0), partition #5 (sda) will be overwritten with random data. It can no longer be recovered after this step has completed. This is the last opportunity to abort the erase.

Really erase the data on SCSI1 (0,0,0), partition #5 (sda) ?

<Go back> <Yes> <No>

[!] Configure the package manager（0.）

Scanning your installation　media finds the label:
Debian GNU/Linux 11.1.0 _Bullseye_ - official amd64 NETINST 20211009-10:

You now have the option of scanning additional media for use by the package manager (apt). Normally these should be from the same set ads the one you booted from.
If you do not have any additional media, this step can just be skipped.
If you wish to scan more media, please insert another one now.

Scan extra installation media?

<Go Back> <Yes> <No>

[!] Configure the package manager（1.）

The goal is to find a mirror of the Debian archive that is close to you on the network — be aware that nearby countries, or even your own, may not be the best choice.

Debian archive mirror country:

[!] Configure the package manager（2.）

Please select a Debian archive mirror. You should use a mirror in your country or region if you do not know which mirror has the best INternet connection to you.

Usually, deb.debian.org is a good choice.

Debian archive mirror:


[!] Configure the package manager（3.）

If you need to use a HTTP proxy to access the outside world, enter the proxy information here.
Otherwise, leave this blank.

The procy information should be given in the standard from of “http://[[user][:pass]@]host[:port]”.

HTTP proxy information (blank for none):

[!] Configure the package manager（4.）

The system may anonymously supply the distribution developers with statistics about the most used packages on this system.
This infomation influences decisions such as which packages should go on the first distribution CD.

If you choose to participate, the automatic submission script will run every week, sending statistics to the distribution developers.
The collected statics can be viewed on https://popcon.debian.org/.

This choice can be later modified by running  “dpkg-reconfigure popularity-contest”.

Participate in the package usage survey?

[!] Software selection (5.)

[!] Install the GRUB boot loader(6.)

It seems that this new installlation is the only oparating system on this computer.
If so, it should be safe to install the GRUB boot loader to your primary drive (UEFI partition/boot record).

Warning: If your computer has another oparating system that the installer failed to detect, this will make that oparating system temporarily unbootable, though GRUB can be manually configured later to boot it.

Install the GRUB boot loader to your primary drive?


[!!] Finish the installation

Installation complete
Installation is complete, so it is time to boot into your new system.
Make sure to remove the installation media, so that you boot into the new system rather than restarting the installation.
