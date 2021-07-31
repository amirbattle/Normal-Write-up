# Normal
The write-up for normal during the ImaginaryCTF

**Category:** Reverse Engineering

**Author:** Amir Battle
# Overview
Normal.v was written in verilog code. After anyalyzing the code, you can see that some variables are assigned very large hex values. These variables are then put into a sequence of nor gates with the final output being the flag. To handle this I wrote a simple python script that emulated the sequence of nor gates in normal.v 

```python
c1 = 0x44940e8301e14fb33ba0da63cd5d2739ad079d571d9f5b987a1c3db2b60c92a3
c2 = 0xd208851a855f817d9b3744bd03fdacae61a70c9b953fca57f78e9d2379814c21
flag = 0x696374667b00000000000000000000000000000000000000000000000000007d

w1 = ((flag | c1) ^ 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
w2 = ((flag | w1) ^ 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
w3 = ((c1 | w1) ^ 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
w4 = ((w2 | w3) ^ 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
w5 = ((w4 | w4) ^ 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
w6 = ((w5 | c2) ^ 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
w7 = ((w5 | w6) ^ 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
w8 = ((c2 | w6) ^ 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
out = ((w7 | w8) ^ 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff)
print('This is the flag in hex: ', hex(out))
```

This was the output of this script:
```
This is the flag in hex:  0x4131315f686121315f7468335f6e33775f6e30726d5f6e30722100
```

 I took this value and used xxd to convert it to its raw form and this gave me the flag.
 ```
 ┌──(kali㉿kali)-[~/ctf_practice]
└─$ echo "4131315f686121315f7468335f6e33775f6e30726d5f6e30722100" | xxd -r -p
A11_ha!1_th3_n3w_n0rm_n0r!
```