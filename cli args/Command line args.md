## sys module
- sys module in Python has argv functionality
- returns a list of all command line arguments provided to the script
- first item on the list is always the name of the script
- all the arguments in the list are string
```
import sys  
args_list = sys.argv  
print(args_list)
```
![[Pasted image 20230324175214.png]]

- for complex set of arguments like the following, sys.argv might not be the best solution as it requires effort from our side
	- assuming that the arguments will be passed in GNU style, which is the following:
```
rsync -a -v --exclude="*.pyc" -B 1024 --ignore-existing 192.168.0.3:/tmp/ ./
```
- optional arguments are introduced by "-" or "--"
	- single hyphen will be followed by a single character -> short option
	- double hyphen followed by multiple chars - > long options
	- the optional arguments may have aditional parameters
- in addition to all these there will be mandatory arguments
- the order of compulsory arguments is very important
- argparse allows us to
	- parse command-line args and options
	- take a variable number of parameters in a single option
	- provide subcommands in our CLIs
- to use argparse, four important steps:
	- import argparse
	- create an argument parser by instantiating ArgumentParser
	- add arguments and options **to the parser** using the .add_argument() method
	- call .parse_args() **on the parser** to get the Namespace of arguments
		- used to create and return an object holding the passed arguments and options
```
import argparse  
parser = argparse.ArgumentParser()  
parser.add_argument("x")  
parser.add_argument("y")  
args = parser.parse_args()  
print(args)
```
![[Pasted image 20230324181510.png]]

