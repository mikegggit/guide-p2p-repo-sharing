GIT collaboration using git daemon
==================================
This article describes a process of collaborating without access to github, for example, when sharing w/ a younger coder who cannot legally have a github account.

There are several ways to accomplish this, however this article will only discuss using the git daemon.

Dependencies
------------
The following is required on all machines involved in code-sharing
- Git is installed
- Git daemon is installed
- You have a repository to collaborate on.


Background
----------
The git daemon provides a network protocol for sharing git repositories from any one machine.

In this topology, each computer can be considered both a server and client.

To act as a server, the computer must be running the git daemon.

Steps
-----
### Enable port forwarding on the network gateway for port 9418
This allows hosts on the internet to communicate w/ machines on a local LAN over port 9418.

### Identify public server ip's
On each server, identify a public ip accessible by other computers.  

An easy way to do this is https://www.whatismyip.com/.

### Ensure the repository being shared is enabled for daemon-based sharing.
Make sure there is a file named git-daemon-export-ok in the .git directory of your repository.

### Start git daemon.
In a directory holding the git repository to be shared, run the following:
git daemon --base-path=. --verbose --export-all --enable=receive-pack

Testing
-------
### Forcing external network access
To test remote access to a server from a local device, you can take the following steps:

On the server network:
 - Start a telnet or netcat listener port with verbose logging turned on.
 ~~~
 nc -l 9418
 ~~~  
 - Ensure setup according to above steps.    

On the device:
 - note the device's ip
 - disable wi-fi
 - turn on a hotspot you can pair the device with for internet access
 - determine the new device ip (it should be different now).
 - try connecting to the server
 ~~~
 nc -v [server ip] 9418
 ~~~



Misc notes
----------
You cannot push to a branch currently checked out on the target server.


Reference
---------
https://railsware.com/blog/taming-the-git-daemon-to-quickly-share-git-repository/

https://git-scm.com/docs/git-daemon
