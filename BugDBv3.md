# BugDB v3

## 1. Flag 1

Used query to list all the bugs and attachments, no attachements whatsoever.

Used attachFile to create a new attachement by passing bugId and contents

Everytime you called this mutation it created a new attachment with filename value containing a md5sum.

Played with modifyAttachment mutation using the attachment ID and new file name

Gave some random file name like `/etc/passwd`

Got error.

```json
{
  "errors": [
    {
      "path": [
        "modifyAttachment"
      ],
      "message": "[Errno 2] No such file or directory: u'attachments//etc/passwd'",
      "locations": [
        {
          "column": 3,
          "line": 2
        }
      ]
    }
  ],
  "data": {
    "modifyAttachment": null
  }
}
```

Hmm, script is trying to write the file to ./attachments/<filename>

Tried `../flag` ok response. (Interestingly once the filepath is broken you can't further modify that attachment id, so switched to using different attachement id )

Need to find a way to view these files.

tried `35.227.24.107/15c8dbd4b5/attachments/test`, `35.227.24.107/15c8dbd4b5/attachments/flag` , `35.227.24.107/15c8dbd4b5/attachments/test2` , `35.227.24.107/15c8dbd4b5/flag` didn't work.

Tried with id `35.227.24.107/15c8dbd4b5/attachments/1`

Baamm! Got the response. 

Now tried pushing XSS and php code in the contents of attach file. XSS worked but of no help , php code was useless. Server is not running php (stupid me, should have checked first).

Used modifyAttachment passing filename value as `../main.py` and read with attachment id, 

Got the main python code. But had no flag.

Read `../requirements.txt` file, found module installed `flask` , `flask_graphql`, `sqlalchemy` , `graphene_sqlalchemy`  , `graphene`

But in main.py code had some more import like 

```python
from model import db_session, Attachment
from schema import schema
```

So they must be local modules.

Used modifyAttachment to change attachment path to  "../schema.py" and read using id nothing in there,

Again used modifyAttachment  attachment path to  "../model.py", found `sqlite:///level18.db` being used. So `level18` should be in local disk.

Used modifyAttachment  attachment path to "../level18.db". Read using the id.

**Got the db contents and the Flag**.

