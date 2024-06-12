*jailbreaking in python* 
https://book.hacktricks.xyz/generic-methodologies-and-resources/python/bypass-python-sandboxes --> the stuff u wan to execute once in
https://gynvael.coldwind.pl/n/python_sandbox_escape --> bypasses
https://ctftime.org/writeup/35612 --> a writeup

`exec` --> takes code lines separate with \n
`eval` --> takes one line and finds the return value
`classes = {}.__class__.__base__.__subclasses__()` --> use this to get the classes (i.e. import)