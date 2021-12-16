# Setting forwarding windows x server to windows system

## Server side configuration
Install VcXSrv in this way:
Just click normally with defaults until windows with Extra settings appears.
Select last checkbox with "Disable access control". When firewall windows security 
alert appears tick also second checkbox with "Public networks..."

Then use instruction from this [post](https://stackoverflow.com/a/6317443).
Follow firewall related steps. Rest of post at the end is WSL2 specific.

At the end of VcXSrv configuration you should see button to saving configuration
into a file. It allows to launch app. Copy it to windows startup.

At the windows side of configuration last step is adding DISPLAY environmental 
variable to windows with value:
	localhost:0.0

## Linux side configuration
Type:
	sudo vi /etc/ssh/sshd_config
Check if X11Forwarding is enabled. If not set value to yes and then
reload sshd by:
	sudo service ssh reload

If you want use clipboard in terminal Vim you need to install vim-gtk3 because
terminal vim out of the box propably don't have clipboard.
	sudo apt install vim-gtk3

You can install xclip which allow copy/paste using clipboard.
	sudo apt install xclip

### WSL2 specific
Add following to ~/.bashrc:
	export DISPLAY=$(awk '/nameserver / {print $2; exit}' /etc/resolv.conf 2>/dev/null):0
	export LIBGL_ALWAYS_INDIRECT=1

## Connecting through ssh with windows x server forwarding
For connecting with OpenSsh add -Y option which enables trusted X11 forwarding.
If errors appears add -v argument which will add debug messages. Multiple -v options 
debug messages increase verbosity. For example -vvv is the highest verbosity.
