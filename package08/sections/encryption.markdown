---
layout: package-part-ext
part: Documentation
---
{% include /docLinks.markdown %}



For several reasons discussed earlier, you may want to encrypt your file. We can broadly distinguish two types of file encryption: 1) encrypting an entire volume and 2) encrypting individual files. 

Object storage (discussed in Module 6) is very useful for ease of access and data integrity.  Because several copies of your data is kept, availability is very good: The NeCTAR Object Store is geo-distributed across Nodes of the Research Cloud so that availability is not reliant on any one datacentre or network infrastructure. Only you have access to your object store using your OpenStack credentials, so only you can select files to download, or upload files to a container. However while access to your account is secure, the transfer of your files via the network is not: When you upload and download files, it happens via an unencrypted connection. At the time when you download or upload your files, and the individual files are wired through the Internet, somebody could catch the data you are just transferring. Also, when your files are replicated across NeCTAR nodes, this happens without encryption (Verify! This should be the case though:  [http://docs.openstack.org/security-guide/content/object-storage.html](http://docs.openstack.org/security-guide/content/object-storage.html) Figure 9.2). So the moment of *data transfer *is the critical moment. Extensions to OpenStack (the cloud software that NeCTAR is using) for object storage data encryption have been proposed and will probably be available at some point, however at this point in time you still may still want to encrypt sensitive data before you upload it to the object storage. Because you upload individual files, you should do a per-file encryption. 

If you want to protect your data against breaches (as discussed earlier), you may want to encrypt an entire volume that you have attached to your VM. 

There are many tools for data encryption available. Not all of them support both per-file and whole volume encryption. When choosing a tool for encryption, it is also important that the encryption algorithm used by the tool is secure and cannot be hacked easily. 

## File encryption

In the following we will discuss the following tools which support per-file encryption:

* [GnuPG](#heading=h.l230br6inx6i)

* [AESCrypt](#heading=h.w56b2vd08x2b)

* [Encrypted zip file](#heading=h.bisq9w7wv75l)

* [Mac: Disk Utility](#heading=h.hafy7oyrb8cu)

Other widely known tools which have recently received bad security audits (e.g. EncFS, OpenSSL, Truecrypt) are not discussed in this course.

TODO: Insert screenshots for all the tools

### GnuPG

GnuPG is an implementation of Pretty Good Privacy (PGP). PGP has excellent security: 

*"To the best of publicly available information, there is no known method which will allow a person or group to break PGP encryption by cryptographic or computational means." *([http://en.wikipedia.org/wiki/Pretty_Good_Privacy#Security_quality](http://en.wikipedia.org/wiki/Pretty_Good_Privacy#Security_quality)). 

GnuPG is open-source and accessible through a variety of different clients and tools. New versions of PGP are released periodically and vulnerabilities are fixed by developers as they come to light. There is a simple command line interface, but there are also many graphical interfaces available which are more popular (see [https://www.gnupg.org/related_software/frontends.html](https://www.gnupg.org/related_software/frontends.html)). 

The official releases ([https://www.gnupg.org/download/](https://www.gnupg.org/download/)) can encrypt 

* files 

* entire volumes

* Disk images

* External drives 

* E-mails

* and more.  

All GnuPG tools support multiple encryption types and ciphers.

GnuPG works very well on almost all 32 and 64 bit platforms. It has mainly been developed for Unix systems, but binaries available for **Windows, OS X, Debian/Ubuntu, Android and more** provided (with no guarantee that binary versions provided are current). Mac users may be interested in GPGSuite: [https://gpgtools.org/gpgsuite.html](https://gpgtools.org/gpgsuite.html) 

GnuPG is easy to use and if you keep your private key and the passphrase secret, it is a very secure way to encrypt files. GnuPG also has the advantage that no passwords will appear in any script files if you use your private key to encrypt files.

### AESCrypt

AESCrypt ([https://www.aescrypt.com/](https://www.aescrypt.com/)) is a free, open-source tool which is available for Mac, Linux and Windows. It provides a secure way to **encrypt individual files**, using the industry standard Advanced Encryption Standard (AES).

AESCrypt is easy to use: On **Windows**, you only right-click on a file, select AES Encrypt (or Decrypt) and enter a password. On a **Mac**, you drag the file into the AES Crypt program and type in the password. On the **Linux** command line, you may use the command aescrypt along with the name of the file and the password.

The AESCrypt website  ([https://www.aescrypt.com/](https://www.aescrypt.com/)) provides an excellent documentation on how to set up and use AESCrypt.

### Encrypted zip file

Zip files can be password-protected, but the standard Zip encryption scheme is extremely weak. If your operating system has a built-in way to encrypt zip files, you probably shouldn’t use it. You should use AES-256 encryption. The tools discussed in the following do support AES-256 encryption.

Zip files are archives containing individual files, so this cannot be used to encrypt entire volumes.

#### Windows: 7-zip

7-zip ([http://www.7-zip.org/](http://www.7-zip.org/)) is great for compressing files and is also a strong file encryption tool. It's free even for commercial use and supports 256-bit AES encryption. The official download is Windows only, but there are also unofficial Linux and OS X versions.

In 7-Zip, you can select files in the Windows Explorer window, right-click and select "7-Zip → Add to archive". You should make sure to select the “Add to archive” option in order to encrypt the file. You will be given the option to set a password, make sure you select one! Alternatively, you can open the 7-Zip application and create the archive from there.

 

*Important note: *7-Zip will create a *7z* archive by default, but you can also choose *Zip*. If you *do* opt to go with Zip, be sure to select the AES-256 encryption method instead of the weaker ZipCrypto method. 

#### Mac: Keka

With Keka ([http://www.kekaosx.com/](http://www.kekaosx.com/)), you compress individual files by dragging and dropping them to Keka in the Dock. You may drop multiple files at once, and they will be compressed as one zip file. You can get Keka from the Mac App Store, here is good documentation on the website on how to use it.

*Important note:* Keka creates encrypted *7z* files.By default, the *7z *files will be encrypted using AES-256, which is secure. If the selected format is *Zip*, Zip 2.0 legacy encryption will be used, so this is not recommended!

#### Linux: p7zip

The file explorer on your Linux desktop can be used to create encrypted *7z* archives. You first need to install the *p7zip-full* package (it may also simply be called* p7zip* on some Linux distributions). Installing *p7zip-full *allows your file explorer encrypt your files or folders in the 7*z *compression format. You may find it in your package manager, e.g. on Ubuntu

*  sudo apt-get install p7zip-full*

Then, open your file explorer, right click on the file and select "Compress…". Be sure to select *7z* as a format! In the window that comes up, expand “Other Options”, tick “encrypt the file list too” and type in your password.

You may also use *p7zip* in the command line. The command is simply called *7z*. Refer to the man pages for usage.

*Important note:* p7zip does not store the owner and group of a file. So if you want to use it for backup purposes (or for other reasons want to preserve owner/group), you should pack your files in a tar archive first, and then encrypt the tar archive with p7zip.

To create a tar archive:

  *tar -cf <tar-file-name>.tar <list of your files>*

To extract a tar archive:

  *tar -xf <tar-file-name.tar>*

#### All platforms: Important note

The older zip encryption is not secure! Several tools can create encrypted zip files (the older, insecure version). For example on Linux or Mac, this can be done via command line:

*zip -0 -e backup.zip <yourfile>*

(-0 means "store only" and don’t compress, this is faster. -e means encrypt archive)

It will ask for the password.

On Windows, you can right-click on a file and select "Compress…". 

This is** not secure**! It was shown to demonstrate how you would use the insecure option.

Use the more secure 7-zip instead!

## Encrypt a volume in Ubuntu

Because we base our instances in this course on Ubuntu, we shall also include instructions on how to encrypt a volume in your Ubuntu virtual machine.

In Module 7, we have seen how we can use disk 2 of our ephemeral storage to store our data (disk 2 is essentially a volume storage which is included with the intance). We have also created a Volume storage and attached it to our instance. In both methods we have *mounted* the disk so that we can access it. This type of disk is suitable for Volume Encryption. You can encrypt the whole block of storage, and all files which are written on it will automatically be encrypted. Files on it will be unreadable to others which may gain access to it, even after you delete your storage (or discard your instance).

Once your drive is encrypted and we have made it accessible to your Ubuntu instance (unlocking it by typing in our password), all programs can access the drive, and Ubuntu takes care of automatic encryption and decryption. You will also be able to copy data from remote (e.g. using the *scp* command): This is because the file server on your instance (e.g. the ssh server) takes care of transferring the files to your remote computer after you have requested them. Programs running on U=ubuntu can read the files from the drive, because Ubuntu automatically takes care of the encryption/decryption in the background. For this reason, everyone who may gain access to your private ssh key and passphrase, and connects to your instance, can also read your files, even though they are encrypted. The volume encryption mainly serves as security in case the physical hard-drive gets stolen, or hacked into from outside your VM -- then, nobody can ready your data, because they would have to type in your volume encryption password first.

TODO: Put in Important Box

Before you decide to encrypt your Volume, you should consider carefully whether you want to do this. While encrypting the data adds security in terms of preservation of privacy, it also incurs new risks: If you ever forget your password, access to your data will be lost forever. It may also introduce difficulties with manual data recovery. You should also take into account that performance of reading and writing to your Volume will degrade with the encryption.

End Important Box

Tipp: You can use the same instructions below to encrypt your external USB Drives from a Ubuntu system. However, note that your USB drive will most likely then only be readable from a Linux system!

In the following we will go through the steps to encrypt a volume on a Ubuntu instance. We will use a standard procedure on Linux to encrypt drives with the Linux Unified Key Setup (LUKS). 

TODO important box

Attention: Following the instructions will erase all data on the volume! If you have any files on it, make sure to back them up first.

end important box

1. Ensure your volume is attached to your instance, but not mounted. 

2. If you have data on the volume, now is a good time to securely erase them. We will talk about securely erasing all data in Module 9. If in doubt, you should use an empty (newly created) volume for this tutorial, then you can skip this step, which may take a long time depending on the size of your volume (Volume 9 explains more about how it can be sped up).  Erase your data with:

3. Now, install the file encryption package cryptsetup

4. Now you can encrypt your Volume.

5. Now you can map your encrypted drive to a virtual drive with the following command (You may replace *MySecureDrive* with any name you want to assign to the drive):

6. Now it is time to format your drive (again, replace *MySecureDrive* with your name, if you have chosen another).

7. That’s almost done! Now we can mount the drive to any directory. In this example, we will create a new one */myMount*, but you can choose any other directory::

8. *[Optional]: *By default, you do not have write permissions on the secondary drive. We discussed how to change write permissions in Module 7. Here a brief reminder: You can do this either with

You are all done! You can now use this drive as normal without worrying about encryption, and your data is securely encrypted.

To unmount and free up the volume, type the following commands:

*   sudo umount /MyMount*

*   sudo cryptsetup luksClose MySecureDrive*

After every reboot of your instance, you will have to unlock the drive by typing in your password and mount the drive again:

*  sudo cryptsetup luksOpen /dev/vdc MySecureDrive*

*  sudo mount /dev/mapper/MySecureDrive /MyMount*
