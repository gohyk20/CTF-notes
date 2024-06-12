## Steganography / file corruption

*links*
https://github.com/JohnHammond/ctf-katana#steganography 
https://www.garykessler.net/library/file_sigs.html
https://www.aperisolve.com/
https://georgeom.net/StegOnline/upload

`exiftool` --> file info
`strings (head)` --> strings inside file
`binwalk` --> finds hidden files
`binwalk --dd='.*' (file)` --> extract the files
`stegsolve` --> for jpegs
`steghide`
`pngcheck` --> for pngs
`strings (file) | grep (string)` --> try this if its a pdf or smt! use to search

*File Hex dumps* --> fix corruption
https://hexed.it/
Add headers or trailers

Note: JPGs have TIFF files behind them

*using steghide*
https://steghide.sourceforge.net/documentation.php
`steghide info` --> usually a passphrase is needed tho

*using stegseek*
`stegseek --seed` --> use to detect if a file has steghide
`stegseek --crack (wordlist)` --> runs the wordlist through steg hide

*Least Significant Bit*
Changing the least significant bit of every byte doesn't change the image by much
https://www.boiteaklou.fr/Steganography-Least-Significant-Bit.html#technical-description
https://daniellerch.me/stego/intro/lsb-en/#information-extraction
For images, it doesnt work with jpegs because of compression i think



## *Network capture*

*links*
https://unit42.paloaltonetworks.com/using-wireshark-exporting-objects-from-a-pcap/

1. using Wireshark - open .pcap files
2. check protocol hierarchy
3. Can filter by ftp, http etc. to see various processes/file transfers

## *Disk imaging / file system analysis*

*Mount the disk file*
`sudo mkdir /mnt/challenge`
`sudo mount -o loop <challenge> /mnt/challenge`

*Use autopsy*
https://www.sans.org/blog/a-step-by-step-introduction-to-using-the-autopsy-forensic-browser/

## PDF
https://blog.didierstevens.com/2008/04/09/quickpost-about-the-physical-and-logical-structure-of-pdf-files/
https://2014.mrmcd.net/fahrplan/system/attachments/2353/original/PDF_Secrets.pdf (this good)

structure
- header
- objects -> can look at the objects in hex editor
- cross reference 
- trailer

possible tools
1. pdf-parser
2. pdfid
3. mutool
*ghost script*
`gs   -o repaired.pdf   -sDEVICE=pdfwrite   -dPDFSETTINGS=/prepress  file.pdf`




## Audio
try audicity and sonic visualiser

*morse code*
probably looks like dots and dashes, use cyberchef

*audacity*
go to drop down with multi-view and waveform and click spectogram

## Log analysis

text file with all the ip addresses and methods etc.
https://docs.oracle.com/cd/E19957-01/816-5666-10/esmonsvr.htm
log --> ip, time, request, status, request_size

staus codes: https://learn.microsoft.com/en-us/troubleshoot/developer/webapps/iis/www-administration-management/http-status-code
tldr 100 means continue 200 is successful 300 is redirect 400 is fail 500 is fail

## AD1
open with FTK imager
file>add evidence item>image file, expand it

*export*
custom content right click

## OSINT

archive.today
crt.sh
https://github.com/cipher387/osint_stuff_tool_collection
https://www.social-searcher.com/

