Web Chroot Manager (WCM) 
==================

Web Chroot Manager (WCM) aims to simplify the management of chroot in Linux.  It's a small easy to use application with a built in wizard which creates cnroot environments suitable for PHP-FPM.

WCM supports adding additional binaries to chroot, updating of chroot with new packages, and generating and comparing blueprints of files contained inside the chroot.

## Limitations

WCM has been designed for Ubuntu 12.04+ LTS.  Debian and CentOS/Redhat support are due with the next release.

WCM assumes that the configuration directory is located at /etc/buckhill-wcm

You may change the CFDIR variable in order to move the configuration directory.

## How to use WCM

Note: Starting WCM without any arguments specified will print help text detailing a list of roles and available options.

WCM has several roles and commands:-

- 1. Wizard
- 2. Create
- 3. Install
- 4. Update Blueprint

## 1. Wizard

The wizard is used to generate configuration files which are later used when creating chroot accounts and sites.

There are three sections:

- Primary account
- Secondary account
- Site

## 1. Wizard - Primary Account

The primary account is the account under which the chroot is installed. Its UID and GID are used by PHP-FPM.

The wizard asks for:

- Username - it is good practice to use short usernames
- DNS server - Chroot will use its own resolver settings. You can use the default server settings or those from a provider.
- Shell - Usually there is no reason to change this.
- Extra binaries which will be installed under chroot. The binaries have to exists on the server.  Full path has to be provided and they have to be separated with a space.

## 1. Wizard - Secondary Account

The account for the site administrator. Access is allowed via SFTP.  It inherits the GID of the primary account.  
Since PHP is also run under the primary account you are free to deny access to the files by changing group permissions.

The wizard asks for:

- Username
- Username of parent (Primary Account) under which it will be created
- Shell - Usually there is no reason to change this.
- Email address - Required for other scripts, such as the WordPress Installer (LPI for Wordpress)

The Primary and Secondary account will be automatically added to primary and secondary groups, which are used 
for SSH access policy enforcing. 

Groups are defined in /etc/buckhill-wcm/general.conf under PRIMARY_GROUP and SECONDARY_GROUP variables.

Groups have to be setup on the server and the SSH server already configured, in order for the SSH access policy to work. 

You may use the linux-package-installer (LPI) to install the SSH server, if required.

## 1. Wizard - Site

This generates a configuration for an Apache vhost which uses the PHP-FPM pool configured for the Primary account. 
The wizard requires the Primary account username.

The wizard asks for:

- Domain name
- Primary account username
- Listen socket (exp. 1.2.3.4:80) or leave * if you unsure
- Vhost alias for the domain

## 2. Create


