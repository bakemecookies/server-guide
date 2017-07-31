# server-guide

*server-guide* is a recipe for deploying simple web applications on [Amazon Web Services](https://aws.amazon.com).
  * **Operating System**: [Ubuntu](https://www.ubuntu.com)
  * **Web Server**: [Vapor](https://vapor.codes)
  * **Database**: [MySQL](http://www.mysql.com)
  * **Language**: [Swift](https://swift.org)

## Server

1. [Log in](https://console.aws.amazon.com/ec2/home) to Amazon EC2.
2. Launch a new instance.
   * **AMI**: Ubuntu Server 16.04 LTS (HVM), SSD Volume Type
   * **Instance Type**: t2.micro
   * **Security Group**: SSH, HTTP, and HTTPS enabled

## DNS

1. [Log in](https://console.aws.amazon.com/route53/home) to Amazon Route 53.
2. Create a DNS `A` record for every desired hosted zone pointing to the IP address of the new server created above.
3. (Recommended) Create a `*.<hosted-zone>` DNS `A` alias record for every `A` record created above.

## SSH

Identity files should be located locally at `~/.ssh/`. Adjust all identity-related steps from here on accordingly if they are located elsewhere or named differently.

1. (Recommended) Create/edit your local SSH configuration at `~/.ssh/config`, i.e.:

   ```
   Host <host-name>
        HostName <ip-address>
        User ubuntu
        IdentityFile ~/.ssh/<key-pair>.pem
   ```
2. Perform an initial SSH into the server: `ssh <server-name>` (short, if configured), or `ssh -i ~/.ssh/<key-pair>.pem ubuntu@<server-name>` (long).

## Ubuntu

1. Change `hostname`: `sudo nano /etc/hostname`, with contents `<server-name>`; then `sudo nano /etc/hosts`, edit the `localhost` line to read `127.0.0.1 localhost <server-name>`; then `sudo hostname <server-name>`.
2. Perform an initial system update: `sudo apt-get update`, then `sudo apt-get dist-upgrade`, then `sudo reboot`.
3. (Recommended) Install and enable [Byobu](http://byobu.co/) (5.106-0ubuntu1): `sudo apt-get install byobu`, then `byobu-enable`.

## MySQL

1. Install the MySQL APT repository: `wget https://dev.mysql.com/get/mysql-apt-config_0.8.7-1_all.deb`, then `sudo dpkg --install mysql-apt-config_0.8.7-1_all.deb`, then `sudo apt-get update`.
2. Install MySQL (5.7.19-1ubuntu16.04): `sudo apt-get install mysql-server`.
3. Install [libmysqlclient-dev](http://packages.ubuntu.com/trusty/libmysqlclient-dev) (5.7.19-1ubuntu16.04): `sudo apt-get install libmysqlclient-dev`.
4. (Recommended) Create/edit the `ubuntu` user's MySQL option file at `~/.my.cnf`, i.e.:

   ```
   [client<database>]
   database='<database>'
   user='<database>'
   password='<password>'
   prompt='<database>> '
   ```

## [Git](http://git-scm.com/)

1. Install Git (2.7.4-0ubuntu1): `sudo apt-get install git`.
2. Use [GitHub](https://github.com)'s [guide](https://help.github.com/articles/managing-deploy-keys) to set up a read-only deploy key (email and passphrase are not necessary).
3. Set up a SSH config: `nano ~/.ssh/config`, with contents:

   ```
   Host <repository>
        HostName github.com
        User git
        IdentityFile <deploy-key>
   ```
   then `chmod 644 ~/.ssh/config`.
 4. Install the repository: `git clone <repository>:<repository-owner>/<repository>.git`.

## Vapor

1. Follow the instructions in Vapor's [guide](https://vapor.github.io/documentation/getting-started/install-swift-3-ubuntu.html) to install Vapor (2.0.3).
2. See Vapor's [example](https://github.com/vapor/example) to set up Vapor with [Upstart](http://upstart.ubuntu.com).

---

[![Creative Commons CC0 1.0 Universal Public Domain Dedication](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)
