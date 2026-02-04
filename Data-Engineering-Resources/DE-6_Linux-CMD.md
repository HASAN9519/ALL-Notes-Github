## INDEX  

1. Path, Updating and Installing Packages, Creating and Editing Files
2. Informational Commands, File and directory management commands, Access control commands
3. Display file contents, Customizing view of file content, File & folder archiving and compression
4. Networking commands, Create and execute a basic Shell Script, Using 'shebang' line
5. Pipes, Working with variables, Bash Scripting Advanced, Command substitution
6. Command Line Arguments, Scheduling Jobs using crontab, Conditionals shell scripting

## Special Paths

    ~   # Home directory
    /   # Root directory
    .   # Current directory
    ..  # Parent directory

#### Changing working directory to home directory
    cd ~

#### Changing working directory to parent
    cd ..

#### Changing working directory to root directory
    cd /

#### Changing working directory to child
#### changes current working directory to /bin directory
#### bin directory is called a child of root / directory because it's inside of it
    cd bin     

#### Viewing files in current working directory
    ls

#### Viewing files in any directory
    ls /

#### Display the content of /usr directory
    ls /usr

#### Navigate to /media directory
    cd /media

#### showing contents of /bin directory
#### one of these files in bin is called "ls" because ls command runs using file /bin/ls
    ls /bin

#### run a previous command by pressing up arrow key
#### navigate up and down tree in one command
#### To navigate up to home directory then up to root, enter cd ../.. and 
#### then to navigate down to bin directory, enter /bin
    cd ../../bin

#### finding out default shell, returns path of the shell
    printenv SHELL

#### select shell bash if it's not default
    bash

## Updating and Installing Packages

#### Getting latest packages information
#### apt update doesn't actually update packages, it finds if any packages can be upgraded
    sudo apt update

#### Updating nano, nano is a simple command that enables to use terminal as a text editor
    sudo apt upgrade nano

#### Installing vim, vim is popular text-editing program 
    sudo apt install vim

## Creating and Editing Files

#### Navigating to project directory
#### typing cd /home/pr and pressing TAB will auto-complete cd /home/project path
    cd /home/pr

#### Creating and editing a file
#### This will create a .py (Python) file called my_program.py and enables to begin editing it using nano text editor
    nano my_program.py

#### Type the following to file and Press "CTRL-X" to exit, Press "y" to save and at last Press "ENTER" to confirm file name
    print('Learning Linux is fun!')

#### Running the Python file
#### auto-completing the command by typing python3 my and pressing TAB 
    python3 my_program.py

#### creating python file using vim editor
    vi done.py

#### Press "i" to enter "Insert mode" and Add following contents to the file
    print('I am done with the lab!')

#### Then Press ESC to exit out of "Insert mode" and type ":sav done.py" to save file
#### type ":wq" and press ENTER, here w means write and q means quit 
    python3 done.py

#### run python program located in child directory from home parent directory
    python3 ./project/my_program.py

## Informational Commands

#### Display name of current user
    whoami

#### get a list of currently logged in users
    who

#### basic information about operating system, prints kernel name
    uname

#### Using -a option prints all system information in following order: Kernel name, network node hostname, kernel release date, kernel version, machine hardware name, hardware platform, operating system
    uname -a

#### View kernel version
    uname -r

#### Obtain user and group identity information
    id

#### Get available disk space
    df

#### get available disk space in a "human-readable" format
    df -h

#### View currently running processes
    ps

#### using -e option, can display all of processes running on system includes processes owned by other users
    ps -e

#### information on running processes and system resources
#### top command provides a dynamic, real-time view of system
#### output keeps refreshing until Pressing q or Ctrl+c
    top

#### use -n option to exit automatically after a specified number of repetitions
    top -n 10

#### press following keys with shift while top is running to sort table
#### finding out which process is consuming most memory by entering shift + m
    M	# Memory Usage
    P	# CPU Usage
    N	# Process ID (PID)
    T	# Running Time

#### Display Messages
#### echo command displays given text on screen
#### special characters helps better format output
    \n	# start a new line
    \t	# insert a tab

#### Use -e option of echo command when working with special characters
    echo -e "This will be printed \nin two lines"

#### Display date and time
    date

#### command displays current date in mm/dd/yy format with quote or without
    date "+%D"
    date +%D

#### popular format specifiers
    %d	# Display day of the month (01 to 31)
    %h	# Displays abbreviated month name (Jan to Dec)
    %m	# Displays month of year (01 to 12)
    %Y	# Displays four-digit year
    %T	# Displays time in 24 hour format as HH:MM:SS
    %H	# Displays hour
    %M  # Displays minute
    %S  # Displays second

#### Displaying time in 24 hour format as HH:MM:SS
    date "+%T"

#### man command displays the user manual for any command, press q to quit
    man date

## File and directory management commands

#### Get location of current working directory
    pwd

#### To list all files starting with b in /bin directory
    ls /bin/b*

#### To list all files ending with r in /bin directory
    ls /bin/*r

#### To print list of files with last modified date
    ls -l

#### recursively list all directories and files in current directory tree
    ls -R

#### popular options try with ls command
    -a	# list all files, including hidden files
    -d	# list directories only, do not include files
    -h	# with -l and -s, print sizes like 1K, 234M, 2G
    -l	# include attributes like permissions, owner, size and last-modified date
    -S	# sort by file size, largest first
    -t	# sort by last-modified date, newest first
    -r	# reverse sort order

#### list all files starting with b in /bin directory in reverse sort order
    ls /bin/b* -r

#### To get long listing of all files in /etc, including any hidden files
#### combined the options -l and -a using shorter notation, -la 
    ls -la /etc

#### List files in /etc directory in ascending order of their access time
    ls -ltr /etc

#### creating a directory named scripts in current directory and Use ls command to verify scripts got created or not
    mkdir scripts

#### touch command to create an empty file named myfile.txt and Use ls command to verify file got created or not
    touch myfile.txt

#### If file already exists, touch command updates the access timestamp and date command to verify date change
    date -r myfile.txt

#### Search and locate files
#### find command is used to search for files in a directory
#### search for files can be based on different attributes such as file's name, type, owner, size or timestamp
#### find command conducts a search of entire directory tree starting from given directory name
#### finds all .txt files in sub folders of /etc directory
    find /etc -name '*.txt'

#### find on current directory, iname is to perform a case-insensitive search
    find . -iname '*.txt'

#### Remove files using rm command and press y to confirm deletion or n to cancel
    rm -i myfile.txt

#### Remove folder or directory along with it's child file or folder using -r option
    rm -r folder1

#### to delete Multiple file from same directory, sudo can be used for restricted directory
    rm payment-data.txt tollplaza-data.tsv vehicle-data.csv
    sudo rm payment-data.txt tollplaza-data.tsv vehicle-data.csv

#### rmdir to delete empty directory or folder folder2
    rmdir folder2

#### mv command moves a file from one directory to another
#### Use caution when moving a file because if target file already exists, it will be overwritten by source file
#### if source and target directories are same, mv is used as a rename operation
#### renaming users.txt as user-info.txt
    mv users.txt user-info.txt

#### moving user-info.txt to /tmp directory
    mv user-info.txt /tmp

#### use cp command to copy user-info.txt which is now in /tmp directory to current working directory
    cp /tmp/user-info.txt user-info.txt

#### copies content of /etc/passwd to a file named users.txt under current directory
    cp /etc/passwd users.txt

#### Create a copy of existing file by renaming it in same directory
    cp display.sh report.sh

#### Copy file /var/log/bootstrap.log to current directory
    cp /var/log/bootstrap.log .

#### Copy file backup.sh from current directory to /usr/local/bin/
#### using sudo because usr directory don't have permission to copy or paste files from or into it by default 
    sudo cp backup.sh /usr/local/bin/

#### copy directory or folder /home/project/folder1 to current directory using -r
    cp -r /home/project/folder1 .

## Access control commands

#### Each file and each directory has permissions set for three permission categories: 'owner', 'group' and 'all users'
#### Following permissions are set for each file and directory
    r   # read
    w   # write	
    x   # execute

#### run command ls -ld to see permissions currently set for a directory named final
#### output looks like: drwxr-sr-x 2 theia users 4096 Sep 12 12:48 final
    ls -ld final

#### run command ls -l to see permissions currently set for a file
    ls -l usdoi.txt

#### A sample output looks like: -rw-r--r-- 1 theia theia 8121 May 31 16:45 usdoi.txt
#### permissions set here are rw-r--r--, The - preceding these permissions indicates that usdoi.txt is a file 
#### If it were a directory, there would be a d instead of the -
#### first three entries correspond to the owner, next three correspond to group and last three are for all others
#### owner of file has read and write permissions while user group only has read permissions and all other users have read permission 
#### No users have execute permissions as indicated by - instead of an x in third position for each user category
#### chmod (change mode) command change permissions set for a file

#### change of permissions is specified with help of a combination of the following characters
    r, w and x	# permissions: read, write and execute respectively
    u, g and o	# user categories: owner, group and all others respectively
    +, -	    # operations: grant and revoke respectively

#### removing read permission for all users (user, group and other) on file usdoi.txt and verify with ls -l usdoi.txt command
    chmod -r usdoi.txt

#### To add read access to all users on usdoi.txt
    chmod +r usdoi.txt

#### remove the read permission for 'all other users' category
    chmod o-r usdoi.txt

#### chmod 777 gives all permission to all user
    chmod 777 process_web_log4.py

## Display file contents

#### cat command displays contents of files
    cat usdoi.txt

#### more command displays file contents page by page, Press space bar to display next page
    more usdoi.txt

#### Display first few lines of a file using head command
    head usdoi.txt

#### Print first 3 lines of file
    head -3 usdoi.txt

#### Display last lines of a file
    tail usdoi.txt

#### Print last 2 lines of file usdoi.txt
    tail -2 usdoi.txt

#### find or count number of lines, words and characters in a file 
#### output contains number of lines followed by number of words followed by number of characters in file
    wc usdoi.txt

#### To print only number of lines
    wc -l usdoi.txt

#### To print only number of words
    wc -w usdoi.txt

#### To print only number of characters
    wc -c usdoi.txt

#### Display number of lines in the /etc/passwd file
    wc -l /etc/passwd

## Customizing view of file content

#### View sorted file lines
    sort usdoi.txt

#### To view reverse-sorted lines
    sort -r usdoi.txt

#### View with repeated & consecutive lines merged into one
    uniq zoo.txt

#### Extract lines matching specified criteria
#### grep command search for lines from the input text for specified pattern
#### printing all lines in file usdoi.txt which contain word people
    grep people usdoi.txt

#### frequently used options for grep are:
    -n	# Along with matching lines also print line numbers
    -c	# Get count of matching lines
    -i	# Ignore case of text while matching
    -v	# Print all lines which do not contain pattern
    -w	# Match only if pattern matches whole words

#### Prints all lines from /etc/passwd file which do not contain pattern login
    grep -v login /etc/passwd

#### -v option performs an inverted match, means to select non-matching lines
    grep -v "218.30.103.62" /home/project/test/test.txt > /home/project/test/testnew.txt

#### Display lines that contain string 'not installed' in /var/log/bootstrap.log
    grep "not installed" /var/log/bootstrap.log

#### View lines of file with filter applied to each line
#### cut command view lines of a file after a filter is applied to each line
#### using cut with -c option to view first two characters of each line
    cut -c -2 zoo.txt

#### viewing each line starting from second character
    cut -c 2- zoo.txt

#### cut command to extract characters 2 to 5 from every line
    cut -c 2-5 top-sites.txt

#### cut command to extract characters after field delimiter dot from each line using the "minus f two" option
    cut -d '.' -f2 top-sites.txt

#### View multiple files side by side
#### paste command view multiple files at once with lines being aligned as columns
#### zoo.txt and zoo_ages.txt files are viewed at once using paste
    paste zoo.txt zoo_ages.txt

#### Customizing delimiter by specifying comma instead of default tab
    paste -d "," zoo.txt zoo_ages.txt

#### sed command has -i option which allows editing files in-place, -i option takes an optional extension argument in case want to save original file as a backup with that extension, If no extension is given backup will be skipped
#### matching lines using regex /71.212.224.97/ and then deleting matching lines with 71.212.224.97 using d operator
    sed -i '/71.212.224.97/d' testnew.txt

## File & folder archiving and compression

#### zip compresses files prior to bundling them
#### tar with -z achieves compression by applying g zip on entire tarball but only after bundling it
#### tar command pack multiple files and directories into a single archive file
#### options used with tar are as follows:
    -c	# Create new archive file
    -v	# Verbosely list files processed
    -f	# Archive file name

#### creating an archive of entire /bin directory into a file named bin.tar
    tar -cvf bin.tar /bin

#### To see list of files in archive using -t option in two ways
    tar -tvf bin.tar
    tar -tf bin.tar

#### To untar archive or extract files from archive using -x option in two ways
    tar -xvf bin.tar
    tar -xf bin.tar

#### Using ls command to verify that folder bin is extracted
    ls -l

#### here weblog.tar file or file with path is destination path and test_new.txt file or file with path is source file
    tar -cvf weblog.tar test_new.txt
    tar -cvf /home/project/test/weblog.tar test_new.txt
    tar -cvf /home/project/test/weblog.tar -C /home/project/test/ test_new.txt
    tar -cvf /home/project/test/weblog1.tar -C /home/project/test/ test_new.txt

#### To archive to be compressed, enter command with -czf option
#### It filters archive file through a GNU compression program called g-zip 
#### Adding .gz to output name helps Windows-based programs correctly recognize file type
    tar -czf bin.tar.gz bin

#### To untar .gz files from archive using -x option, last bin is optional destination folder
    tar -xzf bin.tar.gz bin

#### to untar .tgz files from archive using current directory or home directory into another directory
#### tolldata.tgz file is in /home/project/airflow/dags/finalassignment/staging/ source directory
#### untar file in /home/project/airflow/dags/finalassignment/staging or any other destination directory
#### syntax: tar -zvxf directory_of_tgz_file -C destination_directory_to_untar
#### below command is given from current directory or any other except source and destination directory, sudo can be used for restricted directory
    sudo tar -zvxf /home/project/airflow/dags/finalassignment/staging/tolldata.tgz -C /home/project/airflow/dags/finalassignment/staging

#### below code for different destination directory from above
    tar -zvxf /home/project/airflow/dags/finalassignment/staging/tolldata.tgz -C /home/project/test

#### below codes are equivalent for previous two code lines, just using --directory instead of -C
    sudo tar -zvxf /home/project/airflow/dags/finalassignment/staging/tolldata.tgz --directory /home/project/airflow/dags/finalassignment/staging
    tar -zvxf /home/project/airflow/dags/finalassignment/staging/tolldata.tgz --directory /home/project/test

#### To untar .tar files from different source to same or different destination directory, same as above code, some example
    tar -xvf /home/project/airflow/dags/finalassignment/staging/tolldata.tar -C /home/project/test
    tar -xvf /home/project/airflow/dags/finalassignment/staging/tolldata.tar --directory /home/project/test

## Package and compress archive files

#### zip command compress files
#### Zip file top-sites.txt into a file called top-sites.zip
    zip top-sites.zip top-sites.txt

#### Creating a zip file named config.zip consisting of all files with extension .conf in /etc directory
    zip config.zip /etc/*.conf

#### -r option can be used to zip an entire directory
#### Creating an zip file of /bin directory
    zip -r bin.zip /bin

#### Extract, list or test compressed files in a ZIP archive
#### command for listing files of zip archive called config.zip
    unzip -l config.zip

#### command to extract all files in zip archive bin.zip
#### -o option to force overwrite in case running the command more than once
    unzip -o bin.zip

#### extract all files in zip archive important-documents.zip, -DDo used to overwrite and not restore original modified date
    unzip -DDo important-documents.zip

## Networking commands

#### Show system's host name
    hostname

#### -i option to view IP address of host
    hostname -i

#### Test if a host is reachable
#### ping command keeps sending data packets to www.google.com server and prints response it gets back 
#### Press Ctrl+C to stop pinging
    ping www.google.com

#### ping only for limited number of times using -c option
    ping -c 5 www.google.com

#### Display network interface configuration
#### ifconfig command to configure or display network interface parameters for a network
#### To display configuration of all network interfaces of the system
    ifconfig

#### display configuration of an ethernet adapter eth0
#### eth0 is usually primary network interface that connects server to network
    ifconfig eth0

#### Transfer data from or to a server
#### curl command to access file at following URL and display file's contents on screen
    curl https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt

#### curl command to access HTML file and display file's contents on screen
    curl www.google.com

#### writing contents of a URL to a local file
    curl www.google.com -o google.txt

#### access file at given URL and save it in current working directory
    curl -O https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt

#### Downloading file(s) from a URL
#### wget command is similar to curl but it's primary use is for file downloading
#### wget can recursively download files at a URL
    wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0250EN-SkillsNetwork/labs/Bash%20Scripting/usdoi.txt

## Create and execute a basic Shell Script

#### First create a greet.sh file in file system and save it
#### .sh file is the shell script to install given application or to perform other tasks under Linux and UNIX like operating systems
#### script is executed through Linux terminal
#### greet.sh file will Accept a user name and Print a welcome message to user
#### content of file are inside docstring, docstring is not part of file
    # This script accepts user's name and prints 
    # a message greeting the user
    # Print prompt message on screen
    echo -n "Enter your name :"	  	
    # Wait for user to enter a name, and save entered name into variable 'name'
    read name				
    # Print welcome message followed by name	
    echo "Welcome $name"
    # following message should print on a single line, Hence usage of '-n'
    echo -n "Congratulations! You just created and ran your first shell script "
    echo "using Bash on IBM Skills Network"

#### Executing script after checking permissions for new created file
    ls -l greet.sh

#### If file exists and has read permission, run following command to execute it
#### message 'Enter your name :' appears on screen, Type name and press Enter
    bash greet.sh

## Using 'shebang' line (#!)

#### A shell script is an executable text file in which first line usually has form of an interpreter directive 
#### interpreter directive is also known as a ‘shebang’ directive
#### which command helps to find out path of command bash
    which bash

#### First Open file and add following line at beginning of script
    #! /bin/bash
    # This script accepts user's name and prints 
    # a message greeting the user
    # Print prompt message on screen
    echo -n "Enter your name :"	  	
    # Wait for user to enter a name, and save entered name into variable 'name'
    read name				
    # Print welcome message followed by name	
    echo "Welcome $name"
    # following message should print on a single line, Hence usage of '-n'
    echo -n "Congratulations! You just created and ran your first shell script "
    echo "using Bash on IBM Skills Network"

#### add execute permission for user on greet.sh
    chmod +x greet.sh

#### it's not good idea to grant permissions to a script for all user, groups and others alike
#### It is more appropriate to limit execute permission to only owner
#### removing execute permission for all users
    chmod -x greet.sh

#### Giving permissions only to owner for greet.sh
    chmod u+x greet.sh

#### Executing script
#### . refers to current directory, Executing script greet.sh in current directory
    ./greet.sh

## Pipes

#### pipes are commands in Linux which allow output of one command as input of another
#### Pipes "|" use following format: [command 1] | [command 2] | [command 3] ... | [command n]
#### First create a pets.txt file in file system using following texts in docstring and and save it
    goldfish
    dog
    cat
    parrot
    dog
    goldfish
    goldfish

#### combining two commands in correct order on pets.txt using pipe
    sort pets.txt | uniq

#### tr command only accept "standard input" as input (not strings or filenames)
#### tr replaces characters in input text
#### syntax of tr in following format: tr [OPTIONS] [target characters] [replacement characters]
#### using piping to apply command to strings and file contents
#### using echo in combination with tr to replace all vowels in a string with underscores
    echo "Linux and shell scripting are awesome" | tr "aeiou" "_"

#### replacing all consonants with an underscore using -c option
    echo "Linux and shell scripting are awesome" | tr -c "aeiou" "_"

#### In files, using cat in combination with tr to change all of the text to upper case
    cat pets.txt | tr "[a-z]" "[A-Z]"

#### combining 3 command with pipe
    sort pets.txt | uniq | tr "[a-z]" "[A-Z]"

#### using curl in combination with grep command to extract components of URL data by piping output of curl to grep
#### combination pattern to get current price of BTC (Bitcoin) in USD
#### using a public URL API provided by CoinStats
#### a public API (no key required) which returns some json about the current BTC price in USD
#### Entering following command returns BTC price data displayed as a json object
    curl -s --location --request GET https://api.coinstats.app/public/v1/coins/bitcoin\?currency\=USD

#### json field of "price": [numbers].[numbers]
#### following grep command to extract it from json text
    grep -oE "\"price\"\s*:\s*[0-9]*?\.[0-9]*"

#### details of this statement:
    -o          # command grep to only return matching portion
    -E          # command grep to use extended regex symbols such as ?
    \"price\"   # matches string "price"
    \s*         # matches any number (including 0) of whitespace (\s) characters
    :           # matches :
    [0-9]*      # matches any number of digits (from 0 to 9)
    ?\.         # optionally matches . (this is in case price were an integer)

#### pipe BTC data using curl command to grep statement
#### backslash \ character used after pipe | allows to write expression on multiple lines
    curl -s --location --request GET https://api.coinstats.app/public/v1/coins/bitcoin\?currency\=USD |\
        grep -oE "\"price\":\s*[0-9]*?\.[0-9]*"

#### To get only value of price field and drop "price" label, chaining to pipe the same output to another grep
    curl -s --location --request GET https://api.coinstats.app/public/v1/coins/bitcoin\?currency\=USD |\
        grep -oE "\"price\":\s*[0-9]*?\.[0-9]*" |\
        grep -oE "[0-9]*?\.[0-9]*"

## Working with variables

#### List variables already defined in shell using set
    set

#### Defining shell variable on terminal
#### syntax : ver_name=value, no space around =
    frname="hasan"

#### to access its value using echo and dollar sign $
    echo $frname

#### echoing multiple variable after creating another variable age
    echo $frname $age

#### deleting variable using unset command
    unset frname

#### environment variable has extended scope than shell variable
#### extend any shell variable to environment variable by applying export command
    export frname

#### To list all environment variables using env command
#### checking whether age was exported as environment variable by piping output of env to grep & filtering results using pattern 'fr'
    env | grep "fr"

## Bash Scripting Advanced

#### Meta characters
#### Lines beginning with # (with the exception of #!) are comments and won't be executed
#### Multiple commands can be separated from each other using ; in single command line
    pwd;date

#### * wildcard used in filename expansion
#### * character matches any number of any character in filename patterns By itself matches every filename in a given directory
#### following example lists all files whose name ends with .conf in /etc directory
    ls /etc/*.conf

#### following example lists all files whose name starts with 'b' and ends with '.log' in the directory /var/log
    ls /var/log/b*.log

#### ? wildcard used in filename expansion
#### ? character represents single character in filename pattern
#### following command lists all files whose name starts with any single character followed by grep
    ls /bin/?grep

#### Quoting
#### If any special character has to be treated without their special meaning, needed to quote them
#### Quoting using backslash
#### Backslash removes meaning of special character that follows it
    echo The symbol for multiplication is \*

#### Quoting using single quote (')
#### pair of single quotes removes special meanings of all special characters within them (except another single quote)
    echo 'Following are some special characters in shell - < > ; " ( ) \ [ ]'

#### Quoting using double quote (")
#### pair of double quotes removes special meanings of all special characters within them 
#### except another double quote, variable substitution and command substitution
#### double quote returns variable inside $USERNAME but single quote don't
    echo "Current user name: $USERNAME"

## Command substitution

#### Command substitution is feature of shell helps save output generated by a command in a variable
#### used to nest multiple commands so that innermost command's output can be used by outer commands
#### inner command is enclosed in $() and will execute first
#### Store output of command hostname -i in a variable named $myip
    myip=$(hostname -i)
    echo $myip

#### returns current hostname
    echo "Running on host: $(hostname)"

#### Command substitution can be done using back quote syntax
#### output of command which cat is path to command cat 
#### path is sent to ls -l as an input and show permissions for the file cat in the output
    ls -l 'which cat'

#### I/O Redirection
#### Linux sends output of a command to standard output (display) and any error generated is sent to standard error (display)
#### input required by a command is received from standard input (keyboard)
#### to change these defaults, shell provides a feature called I/O Redirection
#### This is achieved using the following special characters:
    <	# Input Redirection
    >	# Output Redirection
    >>	# Append Output
    2>	# Error Redirection

#### Save network configuration details into a file called output.txt
#### sending output of ifconfig command to file instead of standard output(display)
    ifconfig > output.txt

#### checking out contents of output.txt
    cat output.txt

#### Save output of date command into file output.txt and checking out contents of output.txt
#### previous contents of output.txt were overwritten
#### redirect using > makes contents of target file to overwrite
    date > output.txt
    cat output.txt

#### Storing value of variable named color in file color.txt, first create color variable then store it to file
    color="light green" 
    echo $color > color.txt

#### Append output to a file and checking out contents of newoutput.txt
#### output of uname and date commands appended to file newoutput.txt
    uname -a >> newoutput.txt
    date >> newoutput.txt
    cat newoutput.txt

#### Display contents of file newoutput.txt in all uppercase
#### tr command does not accept file names as arguments but it accepts standard input
#### redirect content of file newoutput.txt to input of tr command and output shows all capital letters
    tr "[a-z]" "[A-Z]" < newoutput.txt

#### Pipes and Filters
#### Command pipeline is feature of shell to combine different unrelated commands so one command's output is sent as input to next command
#### A filter command accepts input from standard input and send output to standard output
#### Count total number of files in current directory
#### combining ls and wc using command pipeline syntax
    ls | wc -l

#### counting of all files whose name starts with 'c' in the /bin directory
    ls /bin/c* | wc -l

#### Find total disk space usage
#### df -h command gives disk usage for all individual filesystems including total usage across server under the head overlay
#### Getting overall disk usage using grep for overlay from output of df -h
    df -h | grep overlay

#### List five largest files
#### -S option of ls command sorts files from largest to smallest
#### sending sorted list through a pipe to head command and outputs list of five largest files from /bin directory
#### du -ah /bin → shows sizes of all files in /bin (human-readable)
#### sort -rh → sorts results by size, largest first (-r reverse, -h human-readable sort)
#### head -n 5 → outputs only the top 5 entries
    du -ah /bin | sort -rh | head -n 5

## Command Line Arguments

#### Command line arguments are convenient way to pass inputs to a script
#### Command line arguments can be accessed inside script as $1, $2 and so on, $1 is first argument, $2 is second argument
#### Creating a simple bash script that handles two arguments
#### Save below code as wish.sh in filesystem
    #! /bin/bash
    echo "Hi $1 $2"
    #$1 is the first argument passed to the script
    echo "$1 is your firstname"
    #$2 is the second argument passed to the script
    echo "$2 is your lastname"

#### Make script executable to everyone
    chmod +x wish.sh

#### Run script with the two arguments
    ./wish.sh hasan zaman

#### Save below code as test1.sh in filesystem and run: sh test1.sh hasan zaman
    #! /bin/bash
    frname=$1
    lsname=$2
    echo "Hi $frname $lsname"
    echo "$frname is your firstname"
    echo "$lsname is your lastname"

#### Find total disk space usage
#### Creating a bash script named dirinfo.sh that takes directory name as an argument and prints total number of directories and number of files in it
#### using find command with -type option lists only files or directories depending upon usage of d switch or f switch respectively
#### command wc -l will count lines
#### Save below code as dirinfo.sh in filesystem
    #! /bin/bash
    dircount=$(find $1 -type d|wc -l)
    filecount=$(find $2 -type f|wc -l)
    echo "There are $dircount directories in the directory $1"
    echo "There are $filecount files in the directory $2"

#### Make script executable to everyone and Running the script with one argument
    chmod +x dirinfo.sh
    ./dirinfo.sh /tmp

#### shell script named latest_warnings.sh that prints latest 5 warnings from /var/log/bootstrap.log file
#### Save below code as latest_warnings.sh in filesystem
    #! /bin/bash
    grep warning /var/log/bootstrap.log|tail -5

#### Make script executable to everyone and Running the script
    chmod +x latest_warnings.sh
    ./latest_warnings.sh



## Scheduling Jobs using crontab

#### Cron is a system daemon used to execute desired tasks in background at designated times
#### A crontab file is a simple text file containing a list of commands meant to be run at specified times and edited using crontab command
#### Each line in crontab file has five time-and-date fields followed by a command, a newline character ('\n') and fields are separated by spaces
#### The five time-and-date fields cannot contain spaces and their allowed values are as follows
    minute (0-59)
    hour(0-23, 0 = midnight)
    day (1-31)
    month (1-12)
    weekday (0-6, 0 = Sunday)

#### List cron jobs
#### -l option of crontab command prints current crontab
    crontab -l

#### Add a job to crontab
#### This will create a new crontab file and are ready to add a new cron job
    crontab -e

#### Scroll down to end of file using arrow keys and Add below line at end of crontab file
    0 21 * * * echo "Welcome to cron" >> /tmp/echo.txt

#### above job specifies that echo command should run when minute is 0 and hours is 21, means job runs at 9.00 p.m every day
#### output of command should be sent to a file /tmp/echo.txt
#### Press Control+X to save changes, Press Y to confirm and Press Enter to come out of editor, Check if job is added to crontab using crontab -l

#### Schedule a shell script
#### creating a simple shell script that prints current time and current disk usage statistics
#### Save below code as disk_usage.sh in filesystem
    #! /bin/bash
    # print the current date time
    date
    # print the disk free statistics
    df -h

#### Verifying script is working or not
    chmod u+x disk_usage.sh
    ./disk_usage.sh

#### Scheduling this script to run everyday at midnight 12:00 (when hour is 0 on 24 hour clock)
#### output of this script to be appended to /home/project/disk_usage.log
#### Edit crontab
    crontab -e

#### Scroll down to end of file using arrow keys and Add below line at end of crontab file
#### Press Control+X to save changes, Press Y to confirm and Press Enter to come out of editor, Check if job is added to crontab using crontab -l
    0 0 * * * /home/project/disk_usage.sh >>/home/project/disk_usage.log

#### Remove current crontab
#### -r option causes current crontab to be removed, Caution: This removes all cron jobs
    crontab -r

#### Verify crontab is removed or not using crontab -l
#### Creating a cron job that runs task date >> /tmp/every_min.txt every minute
#### Edit crontab
    crontab -e

#### Scroll down to end of file using arrow keys and Add below line at end of crontab file
#### Press Control+X to save changes, Press Y to confirm and Press Enter to come out of editor, Check if job is added to crontab using crontab -l
    * * * * * date >> /tmp/every_min.txt

## Conditionals shell scripting

#### Conditionals are ways of telling a script to do something under specific condition(s)
#### must put spaces around conditions in the [ ]
#### Every if condition block must be paired with a fi
#### If Syntax:
    if [ condition ]
    then
        statement
    fi

#### Save below code as if_example.sh in filesystem and verify with cat if_example.sh command
    a=1
    b=2
    if [ $a -lt $b ]
    then
        echo "a is less than b"
    fi

#### run sh if_example.sh, sh instruct terminal to run script if_example.sh using default shell
    sh if_example.sh

#### If-Else Syntax (don't use then for else cases):
    if [ condition ]
    then
        statement_1  
    else
        statement_2  
    fi

#### Save below code as if_else_example.sh in filesystem and run command script: sh if_else_example.sh
    a=3
    b=2
    if [ $a -lt $b ]
    then
        echo "a is less than b"
    else
        echo "a is greater than or equal to b"
    fi

#### Elif statement, elif means "else if", Syntax:
    if [ condition_1 ] 
    then
        statement_1 
    elif [ condition_2 ]  
    then
        statement_2  
    fi

#### Save below code as elif_example.sh in filesystem and run command script: sh elif_example.sh
    a=2
    b=2
    if [ $a -lt $b ]
    then
        echo "a is less than b"
    elif [ $a = $b ]
    then
        echo "a is equal to b"
    else # Here a is not <= b, so a > b
        echo "a is greater than b"
    fi

#### Nested Ifs Syntax:
    if [ condition_1 ]  
    then  
        statement_1  
    elif [ condition_2 ] 
        statement_2
        if [ condition_2.1 ]
        then
            statement_2.1
        fi
    else
        statement_3
    fi

#### Save below code as nested_ifs_example.sh in filesystem and run command script: sh nested_ifs_example.sh
    a=3
    b=3
    c=3
    if [ $a = $b ]
    then
        if [ $a = $c ]
        then
            if [ $b = $c ]
            then
                echo "a, b, and c are equal"
            fi
        fi
    else
        echo "the three variables are not equal"
    fi

#### To simplify a single if-statement, Save below code as nested_ifs_example.sh in filesystem and run command script: sh nested_ifs_example.sh
    a=3
    b=3
    c=3
    if [ $a = $b ] && [ $a = $c ] && [ $b = $c ] # && means "and"
    then
        echo "a, b, and c are equal"
    else
        echo "the three variables are not equal"
    fi

## Declaring Array

#### Declaring Array myArray
    declare -a myArray

#### items can be appended to arrays using syntax: myArray+=($myVariable)
#### appending some values to myArray
    myArray+=("Linux")
    myArray+=("is")
    myArray+=("cool!")

#### echoing or printing myArray
    echo ${myArray[@]}