#LinuxCommands 

The **awk** command is useful for processing structured text data in Linux, allowing you to extract, filter, and transform information efficiently.
### Printing Specific Fields
By default, awk splits each line into fields using whitespaces as the delimiter. These fields can be referred to using $1, $2, etc., where $1 is the first field.
```bash
$ head -n 5 server_logs.txt
2023-08-01 08:15:23 192.168.1.100 GET /index.html 200 
2023-08-01 08:16:45 192.168.1.101 GET /about.html 200 
2023-08-01 08:17:30 192.168.1.102 POST /login.php 302 
2023-08-01 08:18:12 192.168.1.103 GET /products.html 
404 2023-08-01 08:19:05 192.168.1.104 GET /services.html 200

$ awk '{print $3}' server_logs.txt | head -n 5
192.168.1.100 
192.168.1.101 
192.168.1.102 
192.168.1.103 
192.168.1.104
```
Here, **{print $3}** tells awk to print the third field of each line, the output of which is piped to head -n 5 to display the first 5 lines.

```bash
$ '{print $3, $5}' server_logs.txt | head -n 5
192.168.1.100 /index.html 
192.168.1.101 /about.html 
192.168.1.102 /login.php 
192.168.1.103 /products.html 
192.168.1.104 /services.html
```
We can print multiple fields per line as well, separating each with a comma.
### Filtering
We can add filters so that only lines that meet certain conditions are used.
```bash
$ awk '$4 == "POST" {print $0}' server_logs.txt | head -n 10
2023-08-02 05:58:04 192.168.1.189 POST /about.html 304
2023-08-02 07:38:24 192.168.1.103 POST /submit_form.php 200
2023-08-01 08:02:27 192.168.1.194 POST /upload.php 200
2023-08-02 07:52:38 192.168.1.130 POST /submit_form.php 200
2023-08-02 04:10:36 192.168.1.195 POST /upload.php 200
2023-08-01 18:43:46 192.168.1.193 POST /products.html 200
2023-08-01 22:22:46 192.168.1.129 POST /products.html 200
2023-08-02 06:52:14 192.168.1.119 POST /login.php 200
2023-08-01 16:57:50 192.168.1.184 POST /api/data 200
2023-08-01 18:56:24 192.168.1.102 POST /login.php 400
```
Here, **$4 == "POST"** is a pattern/condition that evaluates if the fourth field in the line is equal to the string "POST"