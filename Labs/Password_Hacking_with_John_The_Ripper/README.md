# Password Hacking with John The Ripper

> John Hammond | February 26 2023

---------------

This lab is intended to be ran on a modern Kali Linux virtual machine.

-------------


## Instructions

1. Copy-and-paste a list of usernames to work with.

```bash
USERNAMES=("sophie" "nathaniel" "lauren" "christopher" "seth" "connor" "beverly" "craig" "debra" "sandra" "alice" "roberto" "ivan" "gloria" "nicole" "johnny" "juliana" "amanda" "jerry" "joe" "gilbert" "riley" "stephanie" "troy" "liam" "hector" "nick" "clark" "danna" "perry" "alexis" "stuart" "daisy" "joey" "ron" "janet" "albert" "ben" "ashley" "dan" "shane" "scott" "sandy" "paul" "terry" "marion" "isabel" )
```

2. Create all the user accounts.

```bash
sudo bash -c " \
for user in $USERNAMES; do 
	useradd \$user; 
	password=\$(shuf -n 1 /usr/share/wordlists/fasttrack.txt); 
	echo \$user:\$password | chpasswd; 
	echo Creating user \"\$user\"...; 
done ;

echo 'All done!'
"
```

3. Copy the Linux password file and shadow file into a temporary directory.

```bash
sudo bash -c " \
cp /etc/passwd /tmp/passwd
cp /etc/shadow /tmp/shadow
"
```

4. Unshadow the Linux shadow passwords with John The Ripper to collect password hashes.

```bash
sudo bash -c " \
unshadow /tmp/passwd /tmp/shadow | grep '\$y'| tee /tmp/hashes
"
```

5. Crack the passwords with John The Ripper!

```bash
john /tmp/hashes --wordlist /usr/share/wordlists/fasttrack.txt --format=crypt
```

This may take some time, so once you see some passwords come through and you are satsified, press `Ctrl+C` to stop John The Ripper.

## Cleanup

--------

1. Remove all the created users.

```bash
sudo bash -c " \
for user in $USERNAMES; do 
	userdel \$user; 
	echo Removing user \"\$user\"...; 
done ;

echo 'All done!' 
"
```

2. Remove the setup files from the temporary directory.

```bash
sudo bash -c " \
rm /tmp/passwd /tmp/shadow /tmp/hashes
"
```