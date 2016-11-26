---
excerpt: "I decided to move from Google Authenticator to Authy, so I could have more devices and a backup if I lost my phone"
header:
  overlay_color: "#333"
---
## Migrating from Google Authenticator to Authy

Two Factor Authentication is a neccessity these days and I try to use it wherever I can, but I was concerned still over what happened if I lost my phone and thus access to my 2FA credentials. To get around this using Google Authenticator I used Titanium Backup to backup the APK and data every 24 hours, but this was really clunky and I didn't like that there was no built-in method to backup this data.

Then I discovered Authy, which promises features such as account backups and multi-device usage, such as using a Chrome App to access your 2FA codes when needed. I decided to give it a shot but first I had to copy over my old data from Google Auth.

### Exporting Google Authenticator account info
I found that Google Authenticator did not have a simple way to get code information back out, but after some online searching I found [a post on jcode.me detailing the process](https://jcode.me/migrate-from-google-authenticator-to-authy-android-5/). 

For this, you need to use ADB and have ADB drivers installed for your phone.

With the phone connected:

    adb shell
    su
    
On my phone, I then had to copy over an sqlite3 binary, [found here through XDA](http://forum.xda-developers.com/showpost.php?p=57143465&postcount=15), to my Download folder.

	mount -o remount,rw /system 
    cp /sdcard/Download/sqlite3 /system/xbin
    chmod +x /system/xbin/sqlite3
    mount -o remount,ro /system 
    
Now we can finally access the database:

	sqlite3 /data/data/com.google.android.apps.authenticator2/databases/databases
    
In the database, we select all information from the accounts table:

	select * from accounts;
    
This will spit out a number of account details, we are only concerned with the 3rd field, which is the code used we will use to add the key to Authy.

This can be typed in manually, or you can generate a QR code of the following URL to scan in Authy, where XXXX in the example represents the name of the account being added and XXXX represents the code:
	
    otpauth://totp/XXXX?secret=YYYY
    
With all these steps completed, I then scanned in the QR codes I generated, which prompted me to identify the account associated with the code and in some cases, tweak the service it was for. Unfortunately, not all services had an icon for them, so they are forced to use a generic icon.

	

