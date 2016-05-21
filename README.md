# CrashPlanExchanger

Script to provide automated access to multiple CrashPlan services running on your network. Using this script and
some SSH configuration, it is posisble to administer multuple CrashPlan installs without the need for running the
GUI on those hosts directly.

This is a common problem if you are configuring a CrashPlan service on a Linux/Solaris system or via popular NAS
devices like Drobo.


## Configuration
To configure the CrashPlan Exchanger script:

1. Move your existing `.ui_info` to `.ui_info_default`
2. Create additional configuration files for alternate hosts as `.ui_info_[host-or-IP]`. 
3. Launch the script with alternate systems based on filename.
 
For example, a typical invocation might look like so:

     crashplan 10.0.0.10
     crashplan myserver

The contents of the configuration files will come from the .ui_info files on the original host with some modification.

## Configuration Files
The `.ui_info` files are the only mechanism available to configure CrashPlan on startup. Start with the `.ui_info`
files from the target host, these can be copied to your CrashPlan Exchanger client. Here are the common locations 
for .ui_info files on various operating systems:

     Windows 7+   C:\ProgramData\CrashPlan\.ui_info
     Windows XP   C:\Documents and Settings\All Users\Application Data\CrashPlan\.ui.info
     OS X         /Library/Application Support/CrashPlan/.ui_info
     Linux        /var/lib/crashplan/.ui_info
     Solaris      /var/lib/crashplan/.ui_info

Do not touch the UUID in the .ui_info file, this must be the id from the remote instance. When copied to your local
machine, the filename *must* be modified to match the host. 

For example, if you are currently on your Mac in `/Library/Application Support/CrashPlan/` and are configuring the
Linx client `myserver`:

    scp myserver:/var/lib/crashplan/.ui_info .ui_info_myserver

Once the file is local, update to use a different port number.

       [port],b25fbb7f-f1bb-47d9-945c-c53b123df279,localhost

Modify in this example `.ui_info_myserver` with `[port]` to use for this host locally. Each host should get a 
unique port number locally; recommend starting at 4200 but remember that 4243 is reserved and cannot be used.

In call cases the `host` is parsed from the `.ui_info` filename; for example, for the host `10.0.0.10`
the file would need to be `.ui_info_10.0.0.10`.

     
## Pre-Requisits
This script assumes you have installed CrashPlan 4.7.x on your client and target systems. This will not support
earlier versions nor the later 5.x enterprise versions on either side.