# Essential tools
Understanding the essential tools in Linux is a core competency to master, the essential topics according to Red Hats official website is listed below. Many of the bullet points are note terrificly comprehensive hence will not be explained in greater detail.

- Access a shell prompt and issue commands with correct syntax
- Use input-output redirection (>, >>, |, 2>, etc.)
- Use grep and regular expressions to analyze text
- Access remote systems using SSH
- Log in and switch users in multiuser targets
- Archive, compress, unpack, and uncompress files using tar, star, gzip, and bzip2
- Create and edit text files
- Create, delete, copy, and move files and directories
- Create hard and soft links
- List, set, and change standard ugo/rwx permissions
- Locate, read, and use system documentation including man, info, and files in  usr/share/doc
- Understand and use essential tools

### In-outputs/redirects
When redicecting using ">, >>, | 2>" etc.:
The symbol > will replace whatever is in the redirect file, >> will append at the bottom. 
1 denotes standard output
2 denotes standard error
A common use case is 2>&1 &, send standard error to where ever standard output is being redirected, and then going to /dev/null

### grep
`grep` is used to filter, my favorite is to filter non-case sensitive and display the entire line using `grep -ni`. 
An example could be:
```bash
watch -n 1 "sensors | grep -ni temp1 | awk '{print $2}'"
```
Where we receive the CPU temperature each 1 second, and only the temperature. This could set as a global alias `temperature` for colleges who are not as interested in Linux to still see a live feed of the temperature of the system. 

Regular expression:
```bash
touch abc{1..10}.txt
rm ./abc*
```

### SSH
When accessing a remote system, SSH can be used. 
```bash
ssh -p <port> <username>@<IP>
scp <local/file/path> <username>@<IP>:<path/to/remote/directory>
```

### Switch targets of system
To switch runlevel of the system we can non-persistantly switch with `init <runlevel>`
To see what runlevels there are, we can check the manual for `runlevel`
```bash
0 - poweroff
1 - rescue
2, 3, 4 - multi-user target
5 - graphical target
6 - reboot 
```

To persistantly set target we use the command:
```bash
systemctl set-default <target>
```

### Tar, gzip, bzip2
How to compress:
```bash
# Directory
tar -zcvf file.tar.gz /path/to/dir/

# Single file
tar -zcvf file.tar.gz /path/to/filename

# Instead of using gzip, we can use:
tar -cjvf file.tar.bz2 /home/vivek/data/
# By passing the -j flag
```

How to use tar is mentioned in the manual and the example given is `tar -cvf`, `-c` create new archive, `-v` verbose, `-f` file. 
Then `-z` for gzip, `-j` for bzip2. These options are also mentioned in the manual. 
To extract, use the flag `-x` and `-f` for file. E.i. extract file.

### Soft and hard links
```bash
# Soft link
ln -s <path/to/intended/directory> <link_name>

# Hard link
ln <path/to/intended/file> <link_name>
```

### Create and edit text files
Either use touch to just create the file or use vi to directly type input into the text file.
### Create, delete, copy, and move files and directories
Use:
```bash
mv ./<path>/* <path>
cp abc{1..4}* <path>
rm -R <path/to/directory>
# etc...
```
### List, set, and change standard ugo/rwx permissions
To list permissions, use `ll` or `ls -lta`
To set permissions, use:
```bash
chmod <u,o,g>+-<x,w,r> <file or directory>
# In case of directory and set permissions to all files in directory, add -R flag
```
Flags 
### Locate, read, and use system documentation including man, info, and files in usr/share/doc
An important skill to learn is how to search within the manual, use `/` to search within the manual, look in the related commands section to see if you can find something you're looking for if you've forgotten the command. 
For example, if you've forgotten how to label partitions, look for related commands in mkfs.xfs, then you'll come across xfs_admin and go from there

### sed
Find and replace, here it is important to know your regular expressions by heart.
Replace words:
```bash
# s substitute unix with linux g globally
sed 's/unix/linux/g' file.txt

# To remove empty lines in a file 
sed '/^$/d' file.txt
```
