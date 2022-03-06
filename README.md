## INPT DNS 
DNS Zone , check writeup for more information
### Challenge Description
<br/>
"You can dig deeper" means dig command.
<img src="inptdns.png"/> 
I tried dig requesting TXT records.This says DNS zone so I googled it to find that DNS Zone transfer is sometimes referred through its opcode mnemonic AXFR.SO A zone transfer reveals a lot of information about the domain. that's why we get our flag!
<img src="dig.png"/> 
### 

<br/>

## Palette 
<img src="palette.PNG"/> 

Open the image in the GIMP editor, select the color picker tool in the tools menu, select each box from these 18 boxes by color picker.
The HTML notation in hexadecimal of these colors in 18 box will be:
43ffff  52ffff  49ffff  53ffff  49ffff  53ffff
7bffff  6bffff  36ffff  35ffff  50ffff  38ffff
58ffff  65ffff  61ffff  48ffff  54ffff  7dffff

When the hexadecimal value is converted into ASCII using cyberchef, the output will be:
CÿÿRÿÿIÿÿSÿÿIÿÿSÿÿ{ÿÿkÿÿ6ÿÿ5ÿÿPÿÿ8ÿÿXÿÿeÿÿaÿÿHÿÿTÿÿ}ÿÿ
we delete the characters ÿÿ and we will have our flag: 
CRISIS{k65P8XeaHT}

<img src="gimp.png"/> 

## Login:
First I tried to unpack the apk file given to us using apktool. You can do this with the command:

 ```$apktool d login.apk ```
 ```$grep -R  CRISIS  $Binary file lib/armeabi-v7a/libapp.so matches ```
 - We tried opening the file using Notepad++ and this is what gave us searching the first keyword ***CRISIS***
<img src="login.png"/> 

## The magic of php:
This challenge gives you the source code of the web page 
<?php
if(isset ($_POST['password'])){
    $password = $_POST['password'];
    if(md5($password) == 0e7b1c8e2327e0881bf7058bd533dd22)
    {
        echo "$FLAG</br>";
    }
    else echo "Incorrect Password </br>";
}
?>
<html>
    Enter your password : </br>
    <form action="" method="POST">
<input type="password" name="password"></input>    
<input type="submit" ></input>
</form>
</html> 
- Website : http://18.168.221.53/
And accessing the web page you can insert passwords, but the challenges asks to not use bruteforce on the page, Reading the source code the is a clear condition on the md5($password) it should be the value "0e7b1c8e2327e0881bf7058bd533dd22" to see the flag.
So I got the idea to crack this hash:
```$sudo hashcat -m 0 --force hash rockyou.txt ``` 
<img src="hash1.png"/> 
- what we get is a HEX value 206b6d3831303838 which we enter as a password and the flag gets displayed
- <img src="flag-magic.png"/> 
