# Fill rule


As we said before, in packet encryption, the length of the plaintext often does not meet the requirements, and padding is required. How to padding has a lot of rules.


The common [fill rule] (https://www.di-mgt.com.au/cryptopad.html) is as follows. ** It should be noted that even if the length of the message is an integer multiple of the block size, it still needs to be filled. **


In general, if Padding is found to be incorrect after decryption, an exception is often thrown. We can therefore know if Paddig is correct.


## Pad with bytes all of the same value as the number of padding bytes (PKCS5 padding)



Examples are as follows


```

DES INPUT BLOCK  = f  o  r  _  _  _  _  _

(IN HEX)           66 6F 72 05 05 05 05 05

KEY              = 01 23 45 67 89 AB CD EF

DES OUTPUT BLOCK = FD 29 85 C9 E8 DF 41 40

```



## Pad with 0x80 followed by zero bytes (OneAndZeroes Padding)



Examples are as follows


```

DES INPUT BLOCK  = f  o  r  _  _  _  _  _

(IN HEX)           66 6F 72 80 00 00 00 00

KEY              = 01 23 45 67 89 AB CD EF

DES OUTPUT BLOCK = BE 62 5D 9F F3 C6 C8 40

```



This is actually the same as the padding of md5 and sha1.


## Pad with zeroes except make the last byte equal to the number of padding bytes



Examples are as follows


```

DES INPUT BLOCK  = f  o  r  _  _  _  _  _

(IN HEX)           66 6f 72 00 00 00 00 05

KEY              = 01 23 45 67 89 AB CD EF

DES OUTPUT BLOCK = 91 19 2C 64 B5 5C 5D B8

```



## Pad with zero (null) characters



Examples are as follows


```

DES INPUT BLOCK  = f  o  r  _  _  _  _  _

(IN HEX)           66 6f 72 00 00 00 00 00

KEY              = 01 23 45 67 89 AB CD EF

DES OUTPUT BLOCK = 9E 14 FB 96 C5 FE EB 75

```



## Pad with spaces



Examples are as follows


```

DES INPUT BLOCK  = f  o  r  _  _  _  _  _

(IN HEX)           66 6f 72 20 20 20 20 20

KEY              = 01 23 45 67 89 AB CD EF

DES OUTPUT BLOCK = E3 FF EC E5 21 1F 35 25

```



## 2018 Shanghai University Student Network Security Competition aessss


Sometimes you can attack some improperly used Padding. Here is a topic of the 2018 Shanghai University Student Network Security Competition:


The title script is as follows:


```python

import random

import sys

import string

from hashlib import sha256

import SocketServer

from Crypto.Cipher import AES

from secret import FLAG, IV, KEY





class Task(SocketServer.BaseRequestHandler):

    def proof_of_work(self):

        proof = ''.join(

            [random.choice(string.ascii_letters+string.digits) for _ in xrange(20)])

        # print proof

        digest = sha256(proof).hexdigest()

        self.request.send("sha256(XXXX+%s) == %s\n" % (proof[4:], digest))

        self.request.send('Give me XXXX:')

        x = self.request.recv(10)

        x = x.strip()

        if len(x) != 4 or sha256(x+proof[4:]).hexdigest() != digest:

            return False

        return True



    def pad(self, s):

s + = (256 - only (s)) * chr (256 - only (s))
        ret = ['\x00' for _ in range(256)]

        for index, pos in enumerate(self.s_box):

ret [pos] = s [index]
        return ''.join(ret)



def unpad (self, s):
        ret = ['\x00' for _ in range(256)]

        for index, pos in enumerate(self.invs_box):

ret [pos] = s [index]
return &#39;&#39; .join (right [0: -word (right [-1])])


    s_box = [

        0x63, 0x7C, 0x77, 0x7B, 0xF2, 0x6B, 0x6F, 0xC5, 0x30, 0x01, 0x67, 0x2B, 0xFE, 0xD7, 0xAB, 0x76,

        0xCA, 0x82, 0xC9, 0x7D, 0xFA, 0x59, 0x47, 0xF0, 0xAD, 0xD4, 0xA2, 0xAF, 0x9C, 0xA4, 0x72, 0xC0,

        0xB7, 0xFD, 0x93, 0x26, 0x36, 0x3F, 0xF7, 0xCC, 0x34, 0xA5, 0xE5, 0xF1, 0x71, 0xD8, 0x31, 0x15,

        0x04, 0xC7, 0x23, 0xC3, 0x18, 0x96, 0x05, 0x9A, 0x07, 0x12, 0x80, 0xE2, 0xEB, 0x27, 0xB2, 0x75,

        0x09, 0x83, 0x2C, 0x1A, 0x1B, 0x6E, 0x5A, 0xA0, 0x52, 0x3B, 0xD6, 0xB3, 0x29, 0xE3, 0x2F, 0x84,

        0x53, 0xD1, 0x00, 0xED, 0x20, 0xFC, 0xB1, 0x5B, 0x6A, 0xCB, 0xBE, 0x39, 0x4A, 0x4C, 0x58, 0xCF,

        0xD0, 0xEF, 0xAA, 0xFB, 0x43, 0x4D, 0x33, 0x85, 0x45, 0xF9, 0x02, 0x7F, 0x50, 0x3C, 0x9F, 0xA8,

        0x51, 0xA3, 0x40, 0x8F, 0x92, 0x9D, 0x38, 0xF5, 0xBC, 0xB6, 0xDA, 0x21, 0x10, 0xFF, 0xF3, 0xD2,

        0xCD, 0x0C, 0x13, 0xEC, 0x5F, 0x97, 0x44, 0x17, 0xC4, 0xA7, 0x7E, 0x3D, 0x64, 0x5D, 0x19, 0x73,

        0x60, 0x81, 0x4F, 0xDC, 0x22, 0x2A, 0x90, 0x88, 0x46, 0xEE, 0xB8, 0x14, 0xDE, 0x5E, 0x0B, 0xDB,

        0xE0, 0x32, 0x3A, 0x0A, 0x49, 0x06, 0x24, 0x5C, 0xC2, 0xD3, 0xAC, 0x62, 0x91, 0x95, 0xE4, 0x79,

        0xE7, 0xC8, 0x37, 0x6D, 0x8D, 0xD5, 0x4E, 0xA9, 0x6C, 0x56, 0xF4, 0xEA, 0x65, 0x7A, 0xAE, 0x08,

        0xBA, 0x78, 0x25, 0x2E, 0x1C, 0xA6, 0xB4, 0xC6, 0xE8, 0xDD, 0x74, 0x1F, 0x4B, 0xBD, 0x8B, 0x8A,

        0x70, 0x3E, 0xB5, 0x66, 0x48, 0x03, 0xF6, 0x0E, 0x61, 0x35, 0x57, 0xB9, 0x86, 0xC1, 0x1D, 0x9E,

        0xE1, 0xF8, 0x98, 0x11, 0x69, 0xD9, 0x8E, 0x94, 0x9B, 0x1E, 0x87, 0xE9, 0xCE, 0x55, 0x28, 0xDF,

        0x8C, 0xA1, 0x89, 0x0D, 0xBF, 0xE6, 0x42, 0x68, 0x41, 0x99, 0x2D, 0x0F, 0xB0, 0x54, 0xBB, 0x16

    ]


    invs_box = [

        0x52, 0x09, 0x6A, 0xD5, 0x30, 0x36, 0xA5, 0x38, 0xBF, 0x40, 0xA3, 0x9E, 0x81, 0xF3, 0xD7, 0xFB,

        0x7C, 0xE3, 0x39, 0x82, 0x9B, 0x2F, 0xFF, 0x87, 0x34, 0x8E, 0x43, 0x44, 0xC4, 0xDE, 0xE9, 0xCB,

        0x54, 0x7B, 0x94, 0x32, 0xA6, 0xC2, 0x23, 0x3D, 0xEE, 0x4C, 0x95, 0x0B, 0x42, 0xFA, 0xC3, 0x4E,

        0x08, 0x2E, 0xA1, 0x66, 0x28, 0xD9, 0x24, 0xB2, 0x76, 0x5B, 0xA2, 0x49, 0x6D, 0x8B, 0xD1, 0x25,

        0x72, 0xF8, 0xF6, 0x64, 0x86, 0x68, 0x98, 0x16, 0xD4, 0xA4, 0x5C, 0xCC, 0x5D, 0x65, 0xB6, 0x92,

        0x6C, 0x70, 0x48, 0x50, 0xFD, 0xED, 0xB9, 0xDA, 0x5E, 0x15, 0x46, 0x57, 0xA7, 0x8D, 0x9D, 0x84,

        0x90, 0xD8, 0xAB, 0x00, 0x8C, 0xBC, 0xD3, 0x0A, 0xF7, 0xE4, 0x58, 0x05, 0xB8, 0xB3, 0x45, 0x06,

        0xD0, 0x2C, 0x1E, 0x8F, 0xCA, 0x3F, 0x0F, 0x02, 0xC1, 0xAF, 0xBD, 0x03, 0x01, 0x13, 0x8A, 0x6B,

        0x3A, 0x91, 0x11, 0x41, 0x4F, 0x67, 0xDC, 0xEA, 0x97, 0xF2, 0xCF, 0xCE, 0xF0, 0xB4, 0xE6, 0x73,

        0x96, 0xAC, 0x74, 0x22, 0xE7, 0xAD, 0x35, 0x85, 0xE2, 0xF9, 0x37, 0xE8, 0x1C, 0x75, 0xDF, 0x6E,

        0x47, 0xF1, 0x1A, 0x71, 0x1D, 0x29, 0xC5, 0x89, 0x6F, 0xB7, 0x62, 0x0E, 0xAA, 0x18, 0xBE, 0x1B,

        0xFC, 0x56, 0x3E, 0x4B, 0xC6, 0xD2, 0x79, 0x20, 0x9A, 0xDB, 0xC0, 0xFE, 0x78, 0xCD, 0x5A, 0xF4,

        0x1F, 0xDD, 0xA8, 0x33, 0x88, 0x07, 0xC7, 0x31, 0xB1, 0x12, 0x10, 0x59, 0x27, 0x80, 0xEC, 0x5F,

        0x60, 0x51, 0x7F, 0xA9, 0x19, 0xB5, 0x4A, 0x0D, 0x2D, 0xE5, 0x7A, 0x9F, 0x93, 0xC9, 0x9C, 0xEF,

        0xA0, 0xE0, 0x3B, 0x4D, 0xAE, 0x2A, 0xF5, 0xB0, 0xC8, 0xEB, 0xBB, 0x3C, 0x83, 0x53, 0x99, 0x61,

        0x17, 0x2B, 0x04, 0x7E, 0xBA, 0x77, 0xD6, 0x26, 0xE1, 0x69, 0x14, 0x63, 0x55, 0x21, 0x0C, 0x7D

    ]



    def encrypt(self, msg):

        cipher = AES.new(KEY, AES.MODE_CBC, IV)

        return cipher.encrypt(msg).encode('hex')



    def handle(self):

        if not self.proof_of_work():

            return

        self.request.settimeout(15)

        req = self.request

flag_len = len (FLAG)
        assert(flag_len == 33)

        self.flag = self.pad(FLAG)

        assert(len(self.flag) == 256)



        while True:

            req.sendall(

                'Welcome to AES(WXH) encrypt system.\n1. get encrypted flag.\n2. pad flag.\n3.Do some encrypt.\nYour choice:')

            cmd = req.recv(2).strip()

            try:

                cmd = int(cmd)

            except ValueError:

                cmd = 0

            if cmd == 1:

                enc = self.encrypt(self.flag)

                req.sendall('Here is the encrypted flag: 0x%s\n' % enc)

elif cmd == 2:
                req.sendall('Pad me something:')

self.flag = self.unpad (self.flag) [
                    :flag_len] + req.recv(1024).strip()

                assert(len(self.flag) <= 256)

                self.flag = self.pad(self.flag)

                req.sendall('Done.\n')

elif cmd == 3:
                req.sendall('What do you want to encrypt:')

                msg = self.pad(req.recv(1024).strip())

assert (len (msg) &lt;= 256)
                enc = self.encrypt(msg)

                req.sendall('Here is the encrypted message: 0x%s\n' % enc)

            else:

                req.sendall('Do not lose heart～ ！% Once WXH AK IOI 2019 can Solved! WXH is the first in the tianxia!')

                req.close()

                return





class ThreadedServer(SocketServer.ThreadingMixIn, SocketServer.TCPServer):

    pass





if __name__ == "__main__":

    HOST, PORT = '0.0.0.0', 23333

    print 'Run in port:23333'

    server = ThreadedServer((HOST, PORT), Task)

    server.allow_reuse_address = True

    server.serve_forever()



```



### Analysis


The problem with this problem is in padding. Since there are less than 256 bits to padding, the padding bytes are the missing bytes, but if the plaintext is 256 bytes, then padding is not performed according to the code logic:


```python

def pad(self, s):

s + = (256 - only (s)) * chr (256 - only (s))
        ret = ['\x00' for _ in range(256)]

        for index, pos in enumerate(self.s_box):

ret [pos] = s [index]
        return ''.join(ret)

```



The biggest problem is on the unpad, which is not checked by the unpad, only the last byte is used to determine the number of bytes to fill.


```python

def unpad (self, s):
        ret = ['\x00' for _ in range(256)]

        for index, pos in enumerate(self.invs_box):

ret [pos] = s [index]
return &#39;&#39; .join (right [0: -word (right [-1])])
```



We can control the number of padding bytes removed by tampering with the last byte.


### Use


1. Select choice2 and append `256-33 = 223` bytes so that the current flag does not need to be filled. The last byte appended is set to `chr(256-32)`.


2. The server appends our information to the flag and performs s box substitution, and the result is assigned to the flag variable in the class.


3. We choose choice2 again, here we need to append, the server will take the flag variable in the class for reverse S box replacement and unpad, so according to this unpad algorithm will be the next 224 bytes as the padding removed, the plaintext is left The first 32 bits of the real flag.


4. We enter a character i at this time, then the object encrypted at this time is `flag[:32]+i`.


5. Select choice1 to encrypt the current flag, control i to blast, and if the resulting ciphertext is the same as the original flag encrypted ciphertext, the last byte of the flag is obtained.


6. Bleak byte by byte until all flags are obtained.


Exp is as follows:


```python

# -*- coding: utf-8 -*-

from hashlib import sha256

import socket

import string

import itertools

HOST='106.75.13.64'

PORT=54321

sock = socket.socket (socket.AF_INET, socket.SOCK_STREAM)
sock.connect((HOST, PORT))
def brute_force (pad, shavalue):
    for str in itertools.product(string.ascii_letters + string.digits, repeat=4):

        str=''.join(str)

if sha256 (str + pad) .hexdigest () == shavalue:
            print str

            return str

def choice1():

    sock.send("1\n")

    result=sock.recv(1024).strip()[30:]

    sock.recv(1024).strip()

    return result

def choice2(pad):

    sock.send("2\n")

    sock.recv(1024).strip()

    sock.send(pad+"\n")

    sock.recv(1024).strip()

    sock.recv(1024).strip()

def choice3(str):

    sock.send("3\n")

    sock.recv(1024).strip()

    sock.send(str+"\n")

    result=sock.recv(1024).strip()[33:]

    sock.recv(1024).strip()

    return result

content = sock.recv(1024).strip()

pad=content[12:12+16]

hash=content[33:33+64]

sock.recv(1024).strip()

sock.send(str(brute_force(pad,hash))+"\n")

print sock.recv(1024).strip()

flag_enc=choice1()

flag=""

for i in range(33):

    a = ''.join(['a' for _ in range(223)])

    a = a[:-1] + chr(224+i)

    for c in string.printable:

        print c+flag

        choice2(a)

        choice2(c+flag)

        if choice1() == flag_enc:

            flag=c+flag

            print "success:",flag

            break

```



> flag{H4ve_fun_w1th_p4d_and_unp4d} 


