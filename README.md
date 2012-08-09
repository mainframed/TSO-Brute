   /$$$$$$$$ /$$$$$$   /$$$$$$                      
  |__  $$__//$$__  $$ /$$__  $$                     
     | $$  | $$  \__/| $$  \ $$                     
     | $$  |  $$$$$$ | $$  | $$                     
     | $$   \____  $$| $$  | $$                     
     | $$   /$$  \ $$| $$  | $$                     
     | $$  |  $$$$$$/|  $$$$$$/                     
     |__/   \______/  \______/                      
                                   
   /$$$$$$$  /$$$$$$$  /$$   /$$ /$$$$$$$$ /$$$$$$$$
  | $$__  $$| $$__  $$| $$  | $$|__  $$__/| $$_____/
  | $$  \ $$| $$  \ $$| $$  | $$   | $$   | $$      
  | $$$$$$$ | $$$$$$$/| $$  | $$   | $$   | $$$$$   
  | $$__  $$| $$__  $$| $$  | $$   | $$   | $$__/   
  | $$  \ $$| $$  \ $$| $$  | $$   | $$   | $$      
  | $$$$$$$/| $$  | $$|  $$$$$$/   | $$   | $$$$$$$$
  |_______/ |__/  |__/ \______/    |__/   |________/
						
						Eh TSO Brutus?                                            
						By: Soldier of Fortran
						    (@mainframed767)

TSO Brute - The z/OS TSO/E logon panel brute forcer.
-----------------------------------------------------

Because the logon panel for TSO/E tells you if you have a valid user account vs a 
valid/invalid password, you can enumerate users. Since you can enumerate users 
adding a brute forcer was trivial. 


Modes
-----

This script has two modes:
	- User Enumeration: Given a flat file of usernames it 
	will try to use those usernames to log in. It will keep 
	track of valid usernames and print them all out at the 
	end. TSO only allows characters A-z, 0-9, @, # and $ in 
	its username. Additionally a username cannot start with 
	a number and it must be seven characters or less. This 
	script will skip items that would be invalid TSO ids. You 
	do not need to provide a password file if you're using 
	this mode. This mode is envoked by passing -e or 
	--enumeration.
	
	- Brute Forcer: The brute forcer does the same as the 
	enumerator except it requires a flat file of passwords 
	to use. The same rules for passwords apply except it can 
	start with a number and has a max length of eight instead 
	of seven. This is the default mode of the script. 

This program makes use of x3270 and s3270 to perform much of the heavy lifting, 
using py3270 (included with this script). 


The script comes 2 cosmetic modes modes:
	- Movie Mode: In this mode you can watch script typing 
	commands in a 3270 emulator in real time. Its just like
	 watching a cheesy movie! You

	- Quiet mode: In this mode only valid Usernames/passwords
	are printed to the screen. 

MUST CHANGE
-----------
!!!!!!!!
One key change that MUST be made to the script is within the function Get_TSO_PANEL(). This section of the script is used to get to the TSO/E logon panel with an invalid user ID. This is really easy in some environments (simply typing 'tso' at the first screen and passing a bad user will do it) to more involved in other environments. You will most certainly have to change this section of the script before being able to use it. Future versions might make this a seperate file but as it stands you'll need to edit this script to tailor it to your environment. 

For debugging purposes I've left commented commands to write the screens to an HTML file if you're using a headless server. If you have X access simply use movie mode for debugging. 
!!!!!!!!

Using TSO Brute
---------------


Arguments:
  -h, --help            show the help message and exits
  -t, --target TARGET
                        Required: target IP address or Hostname and port: TARGET[:PORT]
                        default port is 23
  -s, --sleep SLEEP
                        Seconds to sleep between actions (increase on slower
                        systems). The default is 1 second. If you find the keyboard 
			lock error occurring make this higher (to 4 or 5 seconds)
  -u, --userfile USERFILE
                        Required: File containing list of usernames
  -p, --passfile PASSFILE
                        File containing list of passwords
  -m, --moviemode       Enables ULTRA AWESOME Movie Mode. Watch the system get
                        hacked in real time!
  -e, --enumerate       Enables Enumeration Mode Only. Default is brute force
                        mode
  -q, --quiet           Only display found users / found passwords


##### Example Syntax
To just enumerate users in quiet mode:

./TSO_Brute.py -t 10.10.10.10:3270 --enumerate -q -u usernames.txt

To enumerate users and then brute force the password for the found user ID, using movie mode and sleep 3 seconds between actions:

./TSO_Brute.py -t 10.10.10.10 -m -s 3 -u usernames.txt -p passwords.txt


Different Operating Systems
---------------------------
#####Linux Users:
	To use this script you'll need to install s3270/x3270 to /usr/bin. 
	On Debian (or other OSes that user apt, like Ubuntu) you can easily 
	install each like so:
		sudo apt-get install s3270 x3270
	once installed it should work just fine.

#####Mac Users:
	Included on github is the pre-compiled s3270/x3270 for Mac OS X Lion 
	(source available from http://x3270.bgp.nu/download.html). 

#####Windows Users:
	Windows support is in Alpha state using WC3270.exe. Getting to the 
	TSO/E logon panel has been sporadic. Testers welcome. 

Known Issues:
-------------
	- If an account is locked out it may report it as password found. This 
	is due to me not having access to a locked out account, yet. 
	
	- There's one peice left to implement: keeping track of how many invalid 
	logon attempts until an account got locked and stopping from exceeding 
	that value.  
