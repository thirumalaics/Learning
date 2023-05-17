- \_\_dict\_\_  is a dictionary or mapping object used to store an obj's writable attrs
- every obj in python has an attr which is denoted by \_\_dict\_\_
	- contains all the attrs defined for the obj
	- called the mapping proxy
```
class fn:
    def __init__(self):
        self.s = 'sadad'
    def nn(self):
        print(self.s)
print(fn.__dict__)
```
![[Pasted image 20230515182856.png]]
```
print(type(fn.__dict__))
print(type(dict.__dict__))
```
![[Pasted image 20230515183110.png]]
```
f = fn()
print(f.__dict__) # dict is returned only with attrs for the obj
```
![[Pasted image 20230515183319.png]]

## vars()
- takes an object as an arg
	- not mandatory
- returns the \_\_dict\_\_ attr of the obj
- if no args is specified for the vars() fn it acts as pythons locals() method

## locals()
- returns a dictionary with all the local variables and symbols for the current program
- returns dict of the current local symbol table
- symbol table is a data structure maintained and constructed by the python compiler
	- contains all of the essential info about each identifier found in the source code of the program
- does not take any arguments
- python combiler maintains a symbol table which contains the necessary information about the program being written
- there are two types of symbol tables in python
	- local
	- global
- local ST stores all the info related to the program's local scope
	- ex: within a class or a method
	- each scope has its own ST
	- we can access this symbol table with the locals() method

 ![[Pasted image 20230516183610.png]]
![[Pasted image 20230516183314.png]]
-  locals dict should not be modified
	- changes will not affect the values of the local and free vars used by the interpreter
![[Pasted image 20230516184042.png]]
- in the above case, no change was made to the actual present variable
- local symbol table is the same as the global symbol table in the case of the global env
![[Pasted image 20230516184853.png]]