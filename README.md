# server_tutorial_test
Server Setup and Hardening: Ubuntu: 16.04 LTS


# Server Setup and Hardening: Ubuntu: 16.04 LTS [ Part 1 ]

## Step One: Initial Setup:

The first task we will tackle is to update the package lists. Run the following command in your terminal: <br>

<pre><span style="color:red;">root@localhost:~#</span> apt-get update</pre>  

Next, now that we have updated our package lists by downloading the latest versions, we will install the upgrades by running the following command in your terminal:
<br>
<pre><span style="color:red;">root@localhost:~#</span> apt-get upgrade</pre>  
You may be prompted to fill in some information, hit enter to leave blank or accept the defaults. <br>
<br>

<blockquote style="padding: 8px 0 3px 10px; border-color:gold;">

<h3>Command Definition Details :</h3>

Command Structure:  
`apt-get ... { update | upgrade ... }`   

Breakdown:
- `apt-get` : <br>
Command-Line tool for handling packages, and may be considered the user's "back-end" to other tools using the APT library.  
- `update` : <br>
Option used to resynchronize the package index files from their sources. The indexes of available packages are fetched from the location specified in `/etc/apt/sources.list`.
- `upgrade` : <br>
Option used to install the newest versions of all packages currently installed on the system from the sources enumerated in `/etc/apt/sources.list`.  

For more details, type `man apt-get` in your terminal.
</blockquote>
<br>

## Step Two: Create a Limited User with Sudo Privileges:

In this step, we will add a new user to our server. This step is important because up to this point we've been running all of our commands as the Root User, which in short, is a bad practice that we won't go into right now. <br>

I will create a user named 'guybrush' as an example. You can use whatever username you prefer:

<pre><span style="color:red;">root@localhost:~#</span> adduser guybrush</pre>  

After hitting enter, we will be prompted to enter a <span style="color:turquoise;">password and retype to confirm</span>, and for some <span style="color:orange;">information about the user</span> being created, as seen below: <br>

<blockquote style="margin:none; padding: 8px 0 10px 10px;">
<h3>Note :</h3> 
You will not see any characters displayed in the terminal while typing your password, this is standard for password entry. The keyboard inputs are still being received.  
<br>
</blockquote> <br>

<pre>
Adding user `guybrush' ...
Adding new group `guybrush' (1002) ...
Adding new user `guybrush' (1002) with group `guybrush' ...
Creating home directory `/home/guybrush' ...
Copying files from `/etc/skel' ...
<span style="color:turquoise;">Enter new UNIX password: 
Retype new UNIX password: </span>
passwd: password updated successfully
Changing the user information for guybrush
<span style="color:orange;">Enter the new value, or press ENTER for the default
        Full Name []: Guybrush
        Room Number []: 
        Work Phone []: 
        Home Phone []: 
        Other []: </span>
Is the information correct? [Y/n]</pre>

When we are satisfied with the information we have provided, type `y` and press enter on the keyboard to confirm and move on.

<blockquote style="padding: 8px 0 3px 10px; border-color:gold;">

<h3>Command Definition Details : </h3>

Command Structure:  
`adduser [options] user group`   

Breakdown:
- `adduser` : <br> 
Command-Line tool to add users to the system according to command line options and configuration information in `/etc/adduser.conf`. <br>
- `user` : <br>
If called with one non-option argument and without the --system or --group options, adduser will add a normal user. <br>
- `user group` : <br> 
If called with two non-option arguments, adduser will add an existing user to an existing group.<br>

For more details, type `man adduser` in your terminal.

</blockquote>
<br>

<blockquote style="margin:none; padding: 8px 10px 10px 10px;">
<h3>Note :</h3> 
The next step will add the user we created to the 'sudo' group - which stands for 'Super User Do'. This allows the user to temporarily run commands as the Root User. Skip this step when you are creating a regular user that you don't want to run sensitive commands that could cause damage to the server.
<br>
</blockquote> <br>

For the purposes of this tutorial, we will now give our new user 'Guybrush' privileges to run commands as the Root User. In the terminal, we will type the following command: <br>

<pre><span style="color:red;">root@localhost:~#</span> adduser guybrush sudo</pre> 

That's it! The user 'Guybrush' is now a part of the 'sudo' group.

As we are still the Root User, now would be a good time to switch to our newly created user:   

<pre><span style="color:red;">root@localhost:~#</span> su guybrush</pre> 

We are now logged in as 'Guybrush':

<pre>
<span style="color:lightgreen;">guybrush@localhost</span>:<span style="color:skyblue;">~</span>$</span> whoami
guybrush
<span style="color:lightgreen;">guybrush@localhost</span>:<span style="color:skyblue;">~</span>$</span> pwd
/home/guybrush
</pre> 

<blockquote style="padding: 8px 0 3px 10px; border-color:gold;">

<h3>Command Definition Details : </h3>

Command Structure:  
`su user` <br>
`whoami` <br>
`pwd` <br>

Breakdown:
- `su` : <br> 
The su command is used to become another user during a login session. Invoked without a username, su defaults to becoming the superuser.
- `whoami` : <br>
Print the user name associated with the current effective user ID.
- `pwd` : <br> 
Print the full filename of the current working directory.

</blockquote>
<br>

<blockquote style="margin:none; padding: 8px 10px 10px 10px;">
<h3>Note :</h3> 
You will notice, now that we are no longer the Root User, I emphasized with colour, the changes to the terminal user. Red with a leading '#' denotes the Root User, Green/Blue with a leading '$' denotes a Regular User. Be careful, as this is not a standard, but good practice if you are unsure of who you are is to use the <code>whoami</code> command:  
 

<pre style="margin-top:10px;"><span style="color:red;">root@localhost:~#</span> whoami
root
</pre>

<pre>
<span style="color:lightgreen;">guybrush@localhost</span>:<span style="color:skyblue;">~</span>$</span> whoami
guybrush
</pre>

</blockquote> <br>
