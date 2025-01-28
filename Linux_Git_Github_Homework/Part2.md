## 1. I prepared one for you so that you don't write the website code from scratch 😃Let's download it and use it. 

#### - Download https://github.com/aytitech/public/raw/main/sayfa.tgz and open it.
#### - Enter the onepage folder and perform git initilization.
#### - Then create a public github repo named onepage on github. Make sure there is no [README.md](http://README.md) file in it.
#### - Send this git initilized project to the repo you just created.
#### - Protect the main branch of your github repo and merge to the main branch without 1 person reviewing it.

<details>
<summary>Answer</summary>

```
$ wget https://github.com/aytitech/public/raw/main/sayfa.tgz
$ tar -zxf sayfa.tgz
$ cd onepage
$ git init
$ git add .
$ git commit -m "initilization"
$ git push -u origin main
```
</details>

## 2. Write a script. When this script is run on your server 

#### - First clone the onepage github repository to the /tmp folder with git clone. “This will only work if you are downloading for the first time or if the machine is turned off and on. Because the tmp folder is cleared when the machine is turned off and on. So the script should first check if this folder exists. If there is no folder, let the clone work. If it exists, it will not work.”
#### - Continue with the script and go into this onepage folder and pull the changes from the remote repository to local.
#### - Then in the index.html file, replace ISIM with your own name and EPOSTA with your own email address.
#### - And finally copy all files inside the onepage folder to the /var/www/html folder. If there are files with the same name in this folder, overwrite them.

<details>
<summary>Answer</summary>

```
$ cat > ~/script.sh << EOF
#!/usr/bin/bash

if [ -d /tmp/onepage ]; then
    echo "Proje mevcut"
    cd /tmp/onepage
    git reset --hard origin/main
    git pull
else
    echo "Proje yok. Cloneluyorum"
    git clone https://github.com/0x421F/onepage.git
    cd /tmp/onepage
fi

echo "-------"
echo "index.html içerisinde değişiklik yapıyorum"
sed -i 's/ISIM/Taha/g' index.html && sed -i 's/EPOSTA/tahabaran@hotmail.com/g' index.html
echo "değişikliler tamamlandı"

echo "-------"

echo "dosyaları /var/www/html adresine kopyalıyorum"
sudo cp -r -f ./* /var/www/html/
echo "dosyalar kopyalandı"
EOF


$ chmod +x ~/script.sh
```
</details>


## 3. Set this script to run every 10 minutes on your server. 


<details>
<summary>Answer</summary>

```
$ crontab -e
*/10 * * * * /home/ec2-user/script.sh
```
</details>


## 4. Access your website on the Ec2 instance with a browser like chrome-edge-safari etc. from your own computer and refresh the page refresh a few times. Let a few more of your friends access the same address. This will generate an access log. 

<details>
<summary>Answer</summary>

```
$ curl ec2-3-249-160-21.eu-west-1.compute.amazonaws.com
```
</details>

## 5. Access the access logs of the Apache web server. From these logs, cut out the ip addresses that accessed, sort them from small to large and save the list of unique ones in a file in the /tmp folder. 

<details>
<summary>Answer</summary>

```
$ sudo cut -d ' ' -f1 /var/log/httpd/access_log | sort | uniq >> /tmp/osman.txt
```
</details>


## 6. Write a shell script. Give 3 numbers as parameters to this shell script. So ./script.sh can be run as 14 83 22. Let this script tell us the largest of the 3 numbers given to it.

<details>
<summary>Answer</summary>

```
$ cat > ~/script1.sh << EOF
#!/usr/bin/bash

if (($1>$2 && $1>$3))
   then
      echo "$1 En büyük sayı"
   elif (($2>$1 && $2>$3))
   then
      echo "$2 En büyük Sayı"
   else
      echo "$3 En büyük Sayı"
   fi
EOF

$ chmod +x script1.sh
$ ./script1.sh 
```
</details>


## 7. Write a shell script. When an ip address is entered as a parameter to this script, check the Apache Access logs to see if there is access with that ip address. If not, it says not found. If there is, tell how many times it was accessed. 

<details>
<summary>Answer</summary>

```
$ cat > ~/script2.sh << EOF
#!/usr/bin/bash
if (sudo grep $1 /var/log/httpd/access_log > /dev/null 2>&1)
   then
      echo "$1 erişim logunda var"
   else
      echo "$1 erişim logunda yok"
   fi
EOF
$ chmod +x script1.sh
$ ./script1.sh 
```
</details>