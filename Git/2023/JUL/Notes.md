# 19
- Git: distributed version-control-system
	- git can track changes in files in any folder
	- git has its own file structure
	- every file has a special hash
	- persistent hash map that stores key-value pairs
	- key: hash of the file, value is the content of the file
	- can be used without internet
	- changes can be tracked locally
- GitHub: repository hosting service
	- used primarily by distributed teams that work on the same project

# 20
- git init
	- creates a git repository
	- repo will be initially empty even if directory where we initialize git is not empty and contains files and dirs
	- creates .git folder
	- after initialization of the repo, a branch called master is created and we will automatically land in that branch

# 26
- git stores objects in .git/objects/ folder
- there are diff types of objects
	- blob
		- used to store any file, with any extension
	- tree
		- used to store info about dirs
		- a tree can contain blobs and other trees
	- commit
		- used to store different versions of our project
	- annotated tag
		- persistent text pointer to specific commit
- these types of objects help git in file tracking
- for management of blobs and trees, we will use so-called low-level git commands
	- `git hash-object`
		- create new object
	- `git cat-file`
		- read git objects
	- `git mktree`
		- create new tree object
# 27
- `echo "thiru" | git hash-object --stdin`
	- returns a string of alpha numeric
		- called hash
		- hash of thiru
	- did not create any file
- `echo "thiru" | git hash-object --stdin -w`
	- creates a folder in .git/objects/ and a file inside that folder
		- such that folder_name + file_name = hash

# 28
- structure of git db is similar to JSON
	- key(hash of the object) value store
	- he said the values cannot be same in git as the keys(hash) are generated from the value(content of file)
	- since each key has to be unique, so values also should be
		- as hash function returns same hash for an input

# 02
- in git, first we create hash with file contents, after that git creates corresponding files in objects folder
- hash function is a function that takes any variable length input and creates **fixed** length hash
- hash is generated based on the contents of the file
- if we know a hash but we do not know the input that generates it, we cannot reverse-generate the input
	- such fns are called one way functions
- for higher security and confidentiality, user passwords are usually stored in dbs as hashes

# 09
- how many objects git can store in the same repo
	- 2 ^ 160
# 10
- what are the chances of producing same hash for diff files(hash collision)?
	- almost 0 within a given repository
- git hash-object and shasum provide diff hashes for the same text
```
<hash> = folder_name + file_name
git cat-file -p <hash> :contents of the object
git cat-file -s <hash> :size of the object in bytes
git cat-file -t <hash> :type of the object(4 object types)
```

# 16
- creating a hash object with an existing file
	- it is not necessary that the file should be within the repo
`git hash-object <filename> -w`
- -w option only writes the object to the repo's objects folder
- git repo stores files independently on its own file-sys in the objects dir
	- the file that was used in the above command was deleted, but that did not affect the file created under objects dir
	- so the lifetime of the files in the objects dir are not tied to the original file
- git blobs do not contain details of file names of files that were used to generate the sha
# 17 
- each blob stored in the objects folder has name that is part of the SHA1
	- the file in the objects folder does not store the name of the source file
- filenames that were used as source for creation of git blobs are not stored in git
- details of size and type of each object is stored within each blob
- git generates SHA1 hash based on the input+type+size
	- this is why the sha generated by git hash command and shasum command are different
	- shasum generates hash purely based on the content
- four fields within a git object in git db
- files in the object dir are stored in compressed binary format, normal cat command cannot display the contents
![[Pasted image 20230817165818.png]]