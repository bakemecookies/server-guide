# server-guide

## Server

1. [Log in](https://console.aws.amazon.com/ec2/home) to Amazon EC2.
2. Launch a new instance.
   * **AMI**: [Ubuntu](http://www.ubuntu.com/) Server 14.04 LTS (HVM), SSD Volume Type
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

## [MySQL](http://www.mysql.com/)

1. Install the MySQL APT repository: `wget http://dev.mysql.com/get/mysql-apt-config_0.3.7-1ubuntu14.04_all.deb`, then `sudo dpkg --install mysql-apt-config_0.3.7-1ubuntu14.04_all.deb`, then `sudo apt-get update`.
2. Install MySQL (5.6.26-1ubuntu14.04): `sudo apt-get install mysql-server`.
3. Install [libmysqlclient-dev](http://packages.ubuntu.com/trusty/libmysqlclient-dev) (5.6.26-1ubuntu14.04): `sudo apt-get install libmysqlclient-dev`.

## [Ruby](https://www.ruby-lang.org/en/)

1. Install [RVM](https://rvm.io/) and Ruby: `gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3`, then `\curl -sSL https://get.rvm.io | bash -s stable --ruby`, then `source /home/ubuntu/.rvm/scripts/rvm`.

## [Git](http://git-scm.com/)

1. Use [GitHub](https://github.com)'s [guide](https://help.github.com/articles/managing-deploy-keys) to set up a read-only deploy key.

## [Passenger](https://www.phusionpassenger.com/) and [Nginx](http://nginx.org/)

1. Follow the instructions in [Phusion](http://www.phusion.nl/)'s [guide](https://www.phusionpassenger.com/library/walkthroughs/deploy/ruby/aws/nginx/oss/trusty/install_passenger.html).
2. Enable [Gzip](https://www.gnu.org/software/gzip/): `sudo nano /etc/nginx/nginx.conf`, uncommenting the `gzip` lines.
