# Modify compilation options

```
docker exec -it oj-backend sh

python3 manage.py shell

from options.options import *
print(SysOptions.languages)
```

This is the language and compiler information and compilation options used by the system. It is a copy of `judge/languages.py`. If you only modify the py file, it will not take effect. Need to run

```
SysOptions.reset_languages()
```

Update the database.

After the system is updated, the py file may be overwritten, but the value of the database is still modified. So please back up the modified configuration yourself.

If you want to modify the content and format of this configuration file, please explore it yourself or ask the developer, and write the document later.
