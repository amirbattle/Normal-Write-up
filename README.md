# Normal-Write-up
The write-up for the normal ctf
# Overview
Normal.v was written verilog code. After anyalyzing the code, you can see that some variables are assigned very large hex values. These variables are then put into a sequence of nor gates with the final output being the flag. To handle this I wrote a simple python script that emulated the sequence of nor gates in normal.v 
 