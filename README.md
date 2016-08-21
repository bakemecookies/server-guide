# server-guide

*server-guide* is a recipe for deploying simple web applications on [Amazon Web Services](http://aws.amazon.com/).
  * **Operating System**: [Ubuntu](http://www.ubuntu.com/)
  * **Application Server**: [Passenger](https://www.phusionpassenger.com/)
  * **Web Server**: [nginx](http://nginx.org/)
  * **Database**: [MySQL](http://www.mysql.com/)
  * **Language**: [Ruby](https://www.ruby-lang.org/en/)

## Server

1. [Log in](https://console.aws.amazon.com/ec2/home) to Amazon EC2.
2. Launch a new instance.
   * **AMI**: Ubuntu Server 14.04 LTS (HVM), SSD Volume Type
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
3. (Recommended) Install and enable [Byobu](http://byobu.co/) (5.77-0ubuntu1.2): `sudo apt-get install byobu`, then `byobu-enable`.

## MySQL

1. Install the MySQL APT repository: `wget https://dev.mysql.com/get/mysql-apt-config_0.7.3-1_all.deb`, then `sudo dpkg --install mysql-apt-config_0.7.3-1_all.deb`, then `sudo apt-get update`.
2. Install MySQL (5.7.14-1ubuntu14.04): `sudo apt-get install mysql-server`.
3. Install [libmysqlclient-dev](http://packages.ubuntu.com/trusty/libmysqlclient-dev) (5.7.14-1ubuntu14.04): `sudo apt-get install libmysqlclient-dev`.
4. (Recommended) Create/edit the `ubuntu` user's MySQL option file at `~/.my.cnf`, i.e.:

   ```
   [client<database>]
   database='<database>'
   user='<database>'
   password='<password>'
   prompt='<database>> '
   ```

## Ruby

1. Install [RVM](https://rvm.io/) (1.27.0) and Ruby (2.2.1): `gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3`, then `\curl -sSL https://get.rvm.io | bash -s stable --ruby`, then `source /home/ubuntu/.rvm/scripts/rvm`.

## [Git](http://git-scm.com/)

1. Install Git (1.9.1-1ubuntu0.3): `sudo apt-get install git`.
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

## Passenger and nginx

1. Follow the instructions in [Phusion](http://www.phusion.nl/)'s [guide](https://www.phusionpassenger.com/library/walkthroughs/deploy/ruby/aws/nginx/oss/trusty/install_passenger.html) to install Passenger (5.0.30-1~trusty1) with nginx (1.10.1-8.5.0.30~trusty1).
2. Enable [Gzip](https://www.gnu.org/software/gzip/): `sudo nano /etc/nginx/nginx.conf`, uncommenting the `gzip` lines.

---

[![Creative Commons CC0 1.0 Universal Public Domain Dedication](http://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)
