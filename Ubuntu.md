#CyberPatriot Ubuntu
 
Ubuntu Commands && Flags
  Note: In essence a directory is a folder, flags are case sensitive


sudo apt install packageName
This installs packages. Ex: sudo apt install firefox That installs firefox

sudo apt update && sudo apt upgrade
 this command updates the list of available packages as well as upgrading those packages

cd
 Change directory cd.. Moves you up a directory. To change a directory type in cd then the path of the desired directory Ex: cd Home/User/Downloads that would take you to the Downloads folder

ls
 this means to list the contents of a directory

ls -la
 list all contents of directory. This includes hidden files

vim user/somedir/file.txt
Simply type the editor of your choice and the file path for it and hit enter. This will, in the case of vim specifically, create a new file if one doesn't exist, or any editor will open an existing file of the specified filepath. Note: alternatively, you can also cd into a directory then just type vim or vi and a filename to create a file. Ex: nano dirname/file.txt	Ex2: vim dirname/file.txt	Ex3: gedit dirname/file.txt	Ex4: vi dirname/file.txt

touch filename
This command allows you to make a file. Ex: touch amongus.txt (this creates an amongus.txt file)

echo some string
echo outputs the strings you pass as arguments that can be printed to a file or stdout (standard output) ex: echo im scared of cisco 
OUTPUT stream (stdout): im scared of cisco
Can also use redirect to a file instead of stdout by using a less than sign ( > ) 
Ex2: echo "This is going to a file" > testFile
OUTPUT stream (testfile): This is going to a file
Think of it as an arrow pointing the output where to go. The arrow in this case is pointing that string to the testFile

cat filename
Can view contents of a file and make files
Ex: cat testFile
OUTPUT: This is going to a file
Ex2: cat > newFile
Ex2 makes a file named newFile with no content in it

find /dir -type (dir or file) -name ".fileExtension"
This command finds files, directories, and allows you to perform operations on them. For our purposes, however, you will likely need this command only for finding files if you choose not to download locate. Ex: find /Documents -type f -name "*.txt"
This will display all files in the directory with
The file type .txt
man commandName
This command displays how other commands work. It is your manual for commands.
Ex: man ls
This will tell you the various flags of ls and how ls works
  -y flag
This flag simply lets you skip typing y and enter on the next command. If you typed sudo apt install firefox, the terminal would ask you to hit y or n if you wanna download or not. the -y saves the time typing y and enter would've took.

-f flag
This flag means fix. For example if you downloaded the gcc compiler and your C code isn't running you could use sudo apt install -f to grab the dependencies if there are any missing, therefore fixing gcc and allowing you to run that glorious C code
-V flag
this flag shows the version of something for example firefox -V would return what version of firefox you have installed



chmod -filepermission
This command allows you to change the permissions of a file. Those permissions are w, r, and x. That is write, read, and executable. Ex: chmod amongus.c -rwx
That command makes the file able to be read, written and executed.

Shortcut Hotkeys
Ctrl+l: Clears terminal screen
Ctrl+c: Interrupt signal or copies text
Ctrl+t: creates new tab in browser
Ctrl+tab: switches through windows
Ctrl+w closes tab in browser
Ctrl+Alt+t opens the terminal

Vim Crash Course
Vim works with modes. Realistically you'll only need two so that's what I'm gonna go over.
We need the insert mode and normal mode. Insert mode lets you type and edit the file. Normal mode allows you to get to other modes or exit.
To get into normal mode hit esc
To get into insert mode hit i 
Normal Mode Commands: :wq! (that writes the changes and quits. You can alternatively use :x!) :q! quits the file with no changes. :%d deletes all lines from the file. If you type a / in vim that lets you search. For example /amongus would search for the word amongusThat's all you really need to know to use it functionally. Feel free to add if you learn anything more.

Securing Ubuntu (In order)
Check for and CHANGE WEAK user PASSWORDS (NEVER CHANGE PASSWORD OF USER YOU ARE)
 If the readme specifies to get rid of ssh then use this COMMAND: sudo apt-get purge openssh-server*
Check for unauthorized admins COMMAND: cat /etc/group | grep sudo
Delete Users that aren't authorized COMMAND: sudo userdel -r username
Always update the system COMMAND: sudo apt update && sudo apt dist-upgrade -y
Install all needed software COMMAND: sudo apt install chkrootkit firefox libpam-cracklib vim nmap gufw ufw -y
 sudo apt install -f -y
	Do this to check and fix a missing dependency
COMMAND: gufw 
reject incoming allow outgoing
sudo systemctl enable ufw
		Browswer Configuration
Check if firefox is updated via the ubuntu software store or type the following COMMAND: sudo apt update && sudo apt-get --only-upgrade firefox -y
Open firefox (ff), type about:preferences into search bar hit enter. Now go to settings and privacy. THINGS TO TURN ON: strict, do not track signal: always, delete cookies when ff is closed, show alerts about breached sites,never remember history, block pop up windows, warn when sites try to install add-ons (potential PUP), block dangerous content, block dangerous downloads, warn about unwanted and uncommon software, query ocsp, enable https for ALL windows.

TO TURN OFF: ask to save logins and passwords, autofill logins and passwords, autofill addresses, autofill credit cards, allow ff to send tech data, allow ff to install and run studies, allow ff to send crash reports

			
File Editing


Edit login.defs file COMMAND: sudo vim /etc/login.defs
This file is long so we want to type /PASS_MAX_DAYS then hit enter
Change PASS_MAX_DAYS=90
 PASS_MIN_AGE=10
PASS_MAX_WARN=7 
Now save the file and exit

Edit /etc/ssh/sshd_config 
	PermitRootLogin no 
	usePAM yes
port 4269
	Remove insecure Protocol 1
	X11Forwarding no
Protocol 2
	PermitEmptyPasswords no

	Why? PermitRootLogin has a known vulnerability, usePAM is a backend for authentification (which we want), change port to above 1000 so it's less likely to be fooled around with by outside entities, Protocol 1 has many vulnerabilites and is insecure, X11 forwarding allows you to start remote applications and forward it to a machine which as you can imagine; it brings a whole host of issues, Protocol 2 is way more secure than Protocol 1, PermitEmptyPasswords... do I really have to explain this one?

COMMAND: sudo systemctl restart ssh.service or sudo systemctl restart ssh.service or sudo stop ssh then sudo start ssh
restart ssh with the new things added. This is mostly to make sure we get the points







Editing Pam.d Files && Sudoers.d
ALWAYS DO THESE LAST BECAUSE THEY CAN MESS UP YOUR IMAGE IF DONE WRONG

Use your editor of choice or gedit to edit the file. COMMAND: sudo vim /etc/pam.d/common-passwords or gedit /etc/pam.d/common-passwords
What to append in file:end of the pam_unix.so line type remember=5 minlen=8
After this, at the end of the pam_cracklib.so line add ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-1 Save and exit
Edit /etc/pam.d/common-auths
COMMAND: sudo vim /etc/pam.d/common-auths or gedit /etc/pam.d/common-auths
That command will bring you to the common auths file. Add at EOF: auth required pam_tally2.so deny=5 onerr=fail unlock_time=1800
Save and exit

Edit sudoers.d
COMMAND: sudo EDITOR=vim visudo or sudo visudo
 Make sure %wheel ALL=(ALL) NOPASSWD: ALL is commented (The # symbol is a comment). If NOPASSWD: ALL is ever uncommented then comment it or remove it.
Save and exit



Maybe (Thinking about adding or not): bastille, clamtk
