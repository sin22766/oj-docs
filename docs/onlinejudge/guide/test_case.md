# Test case format

For common problems, the test case file includes two extensions of `in` and `out`, and for Special Judge, there is only one file called `in`.

The file name of each test case must start with a number. For example, if there are two sets of test cases, the file names of common test cases are `1.in, 1.out, 2.in, 2.out`, SPJ The file names are `1.in, 2.in`. Other forms of files are not recognized in the background.

When compressing, please put the files in the root directory of the compressed package, rather than in a folder, for example, the correct format is

```bash
➜ testcase pwd
/tmp/testcase
➜ testcase tree
.
├── 1.in
├── 1.out

0 directories, 2 files
```

The following is wrong,

```bash
➜ testcase pwd
/tmp/testcase
➜ testcase tree
.
├── 1
│ ├── 1.in
│ └── 1.out

1 directories, 2 files
```

Then compress the test cases into a zip

```bash
➜ testcase zip testcase.zip ./{*.in,*.out}
  adding: 1.in (stored 0%)
  adding: 1.out (stored 0%)
```

View the contents of the compressed package

```bash
➜ testcase unzip -v testcase.zip
Archive: testcase.zip
 Length Method Size Ratio Date Time CRC-32 Name
-------- ------ ------- ----- ---- ---- ------ ----
       0 Stored 0 0% 04-28-16 16:27 00000000 1.in
       0 Stored 0 0% 04-28-16 16:27 00000000 1.out
-------- ------- --- -------
       0 0 0% 2 files
```

If it is compressed in the graphical interface, please select the file to be compressed and right-click to compress it directly.

If you think it is compressed correctly, but the file format or the number of files is wrong, please check whether the hidden files are compressed. It may be `$RECYCLE.BIN` in Windows, `.DS_Store` in Mac, and `in Linux. 1.in~` or `.1.in.swp` file.

At the same time, it is recommended to merge test cases into one file as much as possible to reduce the number of test case groups, which will improve the performance of judging questions to a certain extent and reduce the running time of the contestants’ code.
