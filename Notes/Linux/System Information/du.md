The **du** command (*disk usage*) is used for analyzing disk space usage. 
```bash
$ du
0       ./documents/reports
0       ./documents
10240   ./backups
0       ./logs/archive
0       ./logs/system
5120    ./logs/application
5120    ./logs
15360   .
```
Each line shows the disk usage (in KB) and the corresponding directory path.
### Human-Readable Output (-h)
We can convert the sizes to a more human-friendly output:
```bash
$ du -h
0       ./documents/reports
0       ./documents
10M     ./backups
0       ./logs/archive
0       ./logs/system
5.0M    ./logs/application
5.0M    ./logs
15M     .
```
- K stands for Kilobytes, M stands for Megabytes.
- The "." in the path represents the current directory.
- The disk usage of a directory includes all of its subdirectories.
- 