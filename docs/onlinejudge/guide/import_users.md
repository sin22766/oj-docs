# Import users

Currently only supports file import in `csv` format. CSV files can be created with a text editor or with software such as Excel.

It is required that the csv file does not lead, with three columns: username, password, and email. Not leading means that you don’t have to write in the first column (words such as username and password)
Please save it as a file with `UTF-8` encoding, otherwise Chinese characters may be garbled

+ Use a text editor

```bash
$ cat users.csv
user1,password1,test@qduoj.com
user2,passwd2,tt@qduoj.com
```

This will create two users, user1 password is password1, email test@qduoj.com; user2 password passwd2, email tt@qduoj.com

+ Use Excel, etc.

Just fill in the data in order and save as a csv format file when saving:
![users](https://user-images.githubusercontent.com/20637881/33434820-6faa6154-d61b-11e7-9198-4317a71afa43.png)

![save_users](https://user-images.githubusercontent.com/20637881/33434823-703de26c-d61b-11e7-8fd4-ddc7563471a0.png)
