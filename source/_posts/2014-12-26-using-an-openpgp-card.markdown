---
layout: post
title: "Using an OpenPGP Card"
date: 2014-12-26 18:21:52 +0100
comments: true
categories: 
---

I bought an [OpenPGP card](http://g10code.com/p-card.html) earlier this year and created a new GPG key ([2642D337](http://www.derekkozel.com/2642D337.asc)) during IETF 89. I’ve been using that key to sign git commits, ssh, and sign/encrypt significant emails and files. However I locked the card on purpose while experimenting with it and then encountered problems unlocking and reloading it. Since then the card has been taking up space in my wallet. Yesterday I was lounging and decided to look at my GPG setup and get the card working.

<!-- more -->

Werner Koch helpfully supplied [simple instructions](http://lists.gnupg.org/pipermail/gnupg-users/2009-September/037413.html "Resetting an OpenGPG Card") on resetting an OpenGPG 2.0 card on the GnuPG mailing list. The idea is quite simple, completely lock the card by trying incorrect admin and user PINs then terminate and reactivate the card. Unfortunately this is done using gpg-connect-agent which was producing error messages for me.

```
$ gpg-connect-agent --hex
gpg-connect-agent: can&#39;t connect to the agent: IPC connect call failed
```

Daniel at Ozone Solutions has a good post about the basic setup of using the OpenGPG card with Ubuntu and has an update for Ubuntu 14.04 pointing out that there’s an issue with gpg-agent not running correctly. The solution is to ensure that one and only one gpg-agent daemon is running at a time and that the environment is correctly initialized. I added the following block to my .bash_local file which is sourced from `.bashrc`.

``` bash .bash_local
if [ ! -f /tmp/gpg-agent.env ]; then
        killall gpg-agent;
        eval $(gpg-agent --daemon --enable-ssh-support > /tmp/gpg-agent.env);
fi
. /tmp/gpg-agent.env
```

This fixed the gpg-connect-agent IPC error allowing me to successfully run the reset commands on the card.

``` bash
$ gpg-connect-agent --hex < cardresetcommands
```

``` plain cardresetcommands
/hex
scd serialno
scd apdu 00 20 00 81 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 81 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 81 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 81 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 83 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 83 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 83 08 40 40 40 40 40 40 40 40
scd apdu 00 20 00 83 08 40 40 40 40 40 40 40 40
scd apdu 00 e6 00 00
scd apdu 00 44 00 00
/echo card has been reset to factory defaults
```

You should see 69 83 and 90 00 replies amongst the response indicating that the PIN tries have been exhausted and the card has been terminated and reactivated. It took several tries for me to get those responses. You can check the status of the card at any point.

``` bash
$ gpg2 --card-status | grep retry
PIN retry counter : 3 3 3
```

The first counter is the user pin, then reset code, then the admin pin. The script should reduce user and admin to zero, then reset the card returning all three to 3. You can enter the commands by hand as necessary. I also had to run the last two commands by hand. Your mileage may vary. The default pins are 123456 for the user and 12345678 for the admin.

Your name, url of your public key, preferred language, login name, and whether to require a fresh pin entry for every signature can be set using `gpg2 --card-edit`. The various guides referenced here have detailed descriptions of configuring the card and keys so I won’t repeat it. However here’s a [command reference](https://www.gnupg.org/howtos/card-howto/en/ch03s03.html "GPG Card Administration") for the impatient. 

At this point you can load your (sub)key(s) onto the card. [Riseup](https://help.riseup.net/en/security/message-security/openpgp/best-practices "PGP best practices") has a list of tests you should run on your key if you have already generated one and good instructions if you haven’t. My key fails several of the tests due to issues such as subkeys being self-signed using SHA1, however I don’t see this as a critical enough issue for me to regenerate the keys at this point as none of my uses require the highest degree of verification. But it’s always good to know where the weaknesses in your security plan are. At the end is a reference `gpg.conf` file with the recommended settings. Ana Guerrero has posted a succinct and, in my non-expert view, complete [guide](http://ekaia.org/blog/2009/05/10/creating-new-gpgkey/ "Key generation guide") for creating a secure modern key.

Don’t forget to backup your private key somewhere safe. Printed on paper is a good method, but a LUKS encrypted volume is a much more convenient, but still quite secure, method of digitally backing it up. Chris Boots has posted a [nice guide](http://www.bootc.net/archives/2013/06/07/generating-a-new-gnupg-key/ "Secure digital backups") for both generating keys, backing them up, and loading them onto a smartcard. He does miss changing the hashing algorithms though.

If you already have your key generated and wish to use the LUKS backup method, just follow Chris’ guide until you have created the backup volume then instead of generating a new key import your existing one. Then close the volume as Chris describes.

``` bash
$ gpg2 --export-secret-keys {KEYID} > {KEYID}.private.key
$ gpg2 --export {KEYID} > {KEYID}.public.key
$ export GNUPGHOME=/mnt/gpg-key-backup/gnupghome
$ gpg2 --allow-secret-key-import --import *.key
$ unset GNUPGHOME
$ shred *.key
```

The final step that I took was to add a photo to my key. The recommended size is 240x288 and as the image is distributed with the public key it is useful to minimize the filesize. I used Shotwell to crop the image with the above aspect ratio, convert to resize it, and trimage to compress it. The final size was 14KB which under doubles the size of the total key as I have four 4KB keys, even on a mobile connection this shouldn’t be any problem.

``` bash
$ shotwell dkozel_casual.jpg
$ convert dkozel_casual.jpg -resize 240x288 dkozel_casual.jpg
$ trimage -f dkozel_casual.jpg
$ gpg2 --edit-key {KEYID}
...
gpg> addphoto
...
gpg> showphoto
gpg> save
```

### Further References

* https://alexcabal.com/creating-the-perfect-gpg-keypair/
* http://anthon.home.xs4all.nl/rants/2014/setting_up_an_openpgp_smartcard_with_gnupg/
* http://www.narf.ssji.net/~shtrom/wiki/tips/openpgpsmartcard

