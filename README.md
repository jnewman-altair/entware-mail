# msmtp_oauth2_posix_py3k
This project was written to re-enable my entware-based mail server to talk to gmail again. Because entware has limited tools available, the excellent oauth2token from https://github.com/tenllado/dotfiles/tree/master/config/msmtp was modified to use a file-based approach with as much portability as possible. (hence POSIX)
In addition, the google oauth2 script has not been updated to py3k as far as I can tell, so I did that as well.

# Installation and Configuration
First, copy the files to the system in question, by default this is /usr/local/bin

Edit the get_oauth2token shell script to match this path and the path to your configuration files

Next, you must perform step 1 from the comments in the oauth2.py script in order to register oauth2 as an application before using the get_oauth2token script. 
Create a configuration file that matches the account email address and use the output from oauth2 to populate it. Be sure to set permissions on this file to something sane for passwords and token use, such as 600. 

For example, if the CONFIG_PATH in the script is set to /etc/oauth2 and your email address is xxx@gmail.com, follow step 1 and
Create a file /etc/oauth2/xxx@gmail.com with the following key-value pairs populated from the output of step 1:
~~~~
OAUTH_CLIENT_ID=1038[...].apps.googleusercontent.com
OAUTH_CLIENT_SECRET=c_5jZcYLW7VDG7MuLB1KSGw4
OAUTH_REFRESH_TOKEN=1/Yzm6MRy4q1xi7Dx2DuWXNgT6s37OrP_DW_IoyTum4YA
~~~~
This step only needs to be performed once. New tokens are automatically requested if the stored tokens have expired.

Set up your msmtprc as described in https://wiki.archlinux.org/index.php/Msmtp#OAUTH2_Authentication_for_Gmail under the subsection __a wrapper on oauth2.py__

# Entware-specific requirements and configurations
- msmtp, openssl, python3, and ca-certificates must be installed
- You may need to update the msmtp package in order to support the oauthbearer auth option
- modify the paths in the scripts appropriately, e.g. CONFIG_PATH=/opt/etc/oauth2
