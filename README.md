

HOW TO SET UP A C2 FRAMEWORK.

We have several types of C2 frameworks such as Powershell Empire/Starkiller, Armitage, Metasploit, Covenant e.t.c
I’ll just be showing you the steps on how to create the Armitage framework.
To begin, Armitage is a GUI for the Metasploit Framework, and because of this, it has almost all aspects of a standard C2 framework.

How to SetUp Armitage.

First, clone the repository from Gitlab: “git clone https://gitlab.com/kalilinux/packages/armitage.git && cd armitage”.
Next up, build the current release. use this command. “bash package.sh”. After the building process finishes, the release should be in the “./releases/unix/ folder”. You should check and verify that Armitage was able to be built successfully. “cd ./release/unix/ && ls -la”
In the folder, there are two key files.

Teamserver
This is the file that will start the Armitage server that multiple users will be able to connect to. This file takes two arguments:
IP Address
Your fellow Red Team Operators will use the IP Address to connect to your Armitage server.
Shared Password
Your fellow Red Team Operators will use the Shared Password to access your Armitage server.
Armitage
This is the file you will be using to connect to the Armitage Teamserver. Upon executing the binary, a new prompt will open up, displaying connection information and your username (this should be treated as a nickname, not a username for authentication) and password.
How to Prepare your Armitage Environment.

Before you can launch Armitage, It’s necessary to do some few pre-flight checks to ensure Metasploit is configured properly. Armitage relies heavily on Metasploit’s Database functionality.
You have to initialize the database before launching Armitage, you must execute the following commands: “systemctl start postgresql && systemctl status postgresql”
NB: It’s important to note that you cannot be the root user when attempting to initialize the Metasploit Database.
“msfdb — use-defaults delete”

Connecting to Armitage
Using this command “cd /opt/armitage/release/unix && ./teamserver YourIP ******”.
Once your Teamserver is up and running, you can now start the Armitage client.
“cd /opt/armitage/release/unix && ./armitage” This command is used to connect to the Teamserver and displays the GUI to the user.

Key Takeaways:
When operating a C2 Framework, you never want to expose the management interface publicly; You should always listen on a local interface, never a public-facing one. This complicates access for fellow operators. There is an easy solution for this. For operators to gain access to the server, you should create a new user account for them and enable SSH access on the server, and they will be able to SSH port forward TCP/55553. Armitage explicitly denies users listening on 127.0.0.1; this is because it is essentially a shared Metasploit server with a “Deconfliction Server” that when multiple users are connecting to the server, you’re not seeing everything that your other users are seeing. With Armitage, you must listen on your tun0/eth0 IP Address.

Thank you and yh, feel free to reach out if you got questions!

C2 Framework
Armitage


