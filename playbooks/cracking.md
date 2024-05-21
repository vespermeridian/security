# Cracking Wireless Handshakes, Hashes, Passwords, and Files

## Wireless Handshake Cracking

### *MIC (Message Integrity Check) Cracking*
1. Capture a valid 4-way handshake with a tool like airodump-ng by sending deauth fames to force a client to disconnect from the AP. You can then sniff the reconnect.
2. Convert that capture to a format friendly for Hashcat. This can be done online with [cap2hashcat](https://hashcat.net/cap2hashcat) or offline with [hashcat-utils](https://github.com/hashcat/hashcat-utils.git)

Using offline hashcat-utils, nav to your directory where the hashcat-utils binary is located, then run:
```
./cap2hccapx.bin capture_file.cap cracked_mic.hccapx
```

3. Crack the hash from the cracked mic:
```
hashcat -a 0 -m 22000 cracked_mic.hccapx /usr/share/wordlists/rockyou.txt
```

### *WPA Handshake Cracking*
Same initial steps as above, then crack with Hashcat:
```
hashcat -a 0 -m 22000 mic_to_crack.hccapx /usr/share/wordlists/rockyou.txt
```

### *WPA/WPA2-PSK (PMKID Crack)*
The PMKID is located in the 1st packet of the 4-way handshake and can be easier to obtain since it does not require capturing the entire 4-way handshake.

1. Grab the PMKID in the 1st packet of the handshake via airmon
2. Extract it from the capture with hcxpcaptool:
```
hcxpcaptool -z <output file name> <cap file>.cap 
```
3. Crack the hashed output with Hashcat:
```
hashcat -a 0 -m 22000 <hash file> /usr/share/wordlists/rockyou.txt
```

## Hash Cracking

### *Hybrid Hash Crack with Hashcat*
A list of modes for different hash types can be found at: [Hashcat Examples](https://hashcat.net/wiki/doku.php?id=example_hashes)

You can identify hash types with: [Hash Type Identifier Online](https://hashes.com/en/tools/hash_identifier)

Crack a hash with 1000 random rules and a pool of rule operators valid for random rule engine

```
hashcat -a 0 -m 1000 <HASH OR FILE> /home/gibson/tools/SecLists/Wordlists/rockyou.txt --generate-rules=1000 --generate-rules-func-sel=ioTlc
```
## File Cracking

### *Cracking MS Office Files*
| Mode | Target |
| ---- | ------ |
| 9400 | MS Office 2007 |
| 9500 | MS Office 2010 |
| 9600 | MS Office 2013 |

1. Extract hash with Office2John
```
python office2john.py hashcat_Word_example.docx > office_hash.txt
```
2. Crack hash with Hashcat
```
hashcat -m 9600 office_hash.txt /opt/useful/SecLists/Passwords/Leaked-Databases/rockyou.txt
```

### *Cracking Zip Files*
| Mode | Target |
| ---- | ---- |
| 11600	| 7-Zip |
| 13600	| WinZip |
| 17200	| PKZIP (Compressed) |
| 17210	| PKZIP (Uncompressed) |
| 17220	| PKZIP (Compressed Multi-File) |
| 17225	| PKZIP (Mixed Multi-File) |
| 17230	| PKZIP (Compressed Multi-File Checksum-Only) |
| 23001	| SecureZIP AES-128 |
| 23002	| SecureZIP AES-192 |
| 23003	| SecureZIP AES-256 |

1. Crack zip to get hash
```
zip2john test.zip
```
You want to get the actual hash portion and put it in a file. Grab the portion that is between the hash mode identifiers. Example, the portion start inclusive of the identifier: $pkzip$1 and ending with /pkzip$. Example output of the hash here:
```
zip2john test.zip
ver 2.0 efh 5455 efh 7875 test.zip/txt.txt PKZIP Encr: TS_chk, cmplen=23, decmplen=15, crc=301E5838 ts=68AB cs=68ab type=8
test.zip/txt.txt:$pkzip$1*1*2*0*17*f*301e5838*0*41*8*17*68ab*0dafcbdc03bcec1016e2d23b8a1dbaf63fe2f82c153c44*$/pkzip$:txt.txt:test.zip::test.zip
```

The part you want would be:
```
$pkzip$1*1*2*0*17*f*301e5838*0*41*8*17*68ab*0dafcbdc03bcec1016e2d23b8a1dbaf63fe2f82c153c44*$/pkzip$
```
1. Crack hash with Hashcat
```
hashcat -a 0 -m 17200 zip.txt /usr/share/wordlists/rockyou.txt hashcat
```

### *Cracking KeePass Files*

| Mode | Target |
| ---- | ------ |
| 13400 | KeePass 1 AES / without keyfile |
| 13400 | KeePass 2 AES / without keyfile |
| 13400 | KeePass 1 Twofish / with keyfile |
| 13400 | Keepass 2 AES / with keyfile |

1. Extract the hash from the file using keepass2john.py
```
python keepass2john.py Master.kdbx > hash.txt
```
2. Crack the keepass hash with Hashcat
```
hashcat -a 0 -m 13400 keepass_hash /usr/share/wordlists/rockyou.txt
```

### *Cracking PDF Files*
| Mode | Target |
| ---- | ---- |
| 10400 | PDF 1.1 - 1.3 (Acrobat 2 - 4) |
| 10410 | PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #1 |
| 10420 | PDF 1.1 - 1.3 (Acrobat 2 - 4), collider #2 |
| 10500 | PDF 1.4 - 1.6 (Acrobat 5 - 8) |
| 10600 | PDF 1.7 Level 3 (Acrobat 9) |
| 10700 | PDF 1.7 Level 8 (Acrobat 10 - 11) |

1. Extract the hash with pdf2john.py:
```
python pdf2john.py inventory.pdf | awk -F":" '{ print $2}' > hash.txt 
```
2. Crack the hash with Hashcat:
```
hashcat -a 0 -m 10500 hash.txt /usr/share/wordlists/rockyou.txt
```



## Tools / Installation Instructions

### John The Ripper
**C Port Repo:** [John The Ripper](https://github.com/magnumripper/JohnTheRipper/tree/bleeding-jumbo/src)

To Compile:
```
sudo git clone https://github.com/magnumripper/JohnTheRipper.git
cd JohnTheRipper/src
sudo ./configure && make
```
**Python Port Repo:** [John The Repo](https://github.com/magnumripper/JohnTheRipper/tree/bleeding-jumbo/run)

### KeePass
One additional tool ported to Python by @Harmj0y is the keepass2john.py tool for extracting a crackable hash from KeePass 1.x/2.x databases that can be run through Hashcat

**Repo:** [Keepass](https://gist.github.com/HarmJ0y/116fa1b559372804877e604d7d367bbc#file-keepass2john-py)

### Office2John
Hashcat can be used to attempt to crack password hashes extracted from some Microsoft Office documents using the office2john.py tool.

**Repo:** [Office2John.py](https://raw.githubusercontent.com/magnumripper/JohnTheRipper/bleeding-jumbo/run/office2john.py)

```
wget https://raw.githubusercontent.com/magnumripper/JohnTheRipper/bleeding-jumbo/run/office2john.py
chmod +x office2john.py
```
### Zip2John

**Repo:** [Zip2John](https://github.com/magnumripper/JohnTheRipper/blob/bleeding-jumbo/src/zip2john.c)

### KeePass2John
```
wget https://gist.githubusercontent.com/HarmJ0y/116fa1b559372804877e604d7d367bbc/raw/c0c6f45ad89310e61ec0363a69913e966fe17633/keepass2john.py
chmod +x
```
### Hashcat-utils
```
git clone https://github.com/hashcat/hashcat-utils.git
cd hashcat-utils/src
make
```

### hcxpcapngtool (cap cracker)
**Repo:** [hcxTools repo](https://github.com/ZerBea/hcxtools)

```
git clone https://github.com/ZerBea/hcxtools
cd hcxtools/
sudo apt-get install libcurl4-openssl-dev
sudo make install PREFIX=/usr/local
```
