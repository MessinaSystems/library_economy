# library Economy test

# Instructions 
The instructions are based on the link here: https://wiki.koha-community.org/wiki/Koha_on_Debian for koha 24.11 on Ubuntu 22 LTS server
This is an example bike library

## Pre configuration
`sudo apt update && sudo apt upgrade -y`
`sudo apt install sudo apt-transport-https ca-certificates curl -y`

## Add the koha package
`sudo mkdir -p --mode=0755 /etc/apt/keyrings`
`sudo curl -fsSL https://debian.koha-community.org/koha/gpg.asc -o /etc/apt/keyrings/koha.asc`
`sudo apt update`

## Specify koha version 24.11
`echo 'deb [signed-by=/etc/apt/keyrings/koha.asc] https://debian.koha-community.org/koha 24.11 main' | sudo tee /etc/apt/sources.list.d/koha.list`
`sudo apt update`

## Install koha and database
`sudo apt install koha-common mariadb-server -y`

## Configure Koha for DNS
`sudo vi /etc/koha/koha-sites.conf`
## Change .myDNSname.org to .website.com

## Setup Apache as the webserver
`sudo a2enmod rewrite cgi headers proxy_http ssl deflate`
`sudo systemctl restart apache2`

## Remove the default apache2 (if using port 80 for the default site)
`sudo a2dissite 000-default`

## Creating a Koha instance
'sudo koha-create --create-db bikelibrary'
'sudo koha-plack --enable bikelibrary'
'sudo koha-plack --start bikelibrary'
'sudo systemctl restart apache2'

## Get Username and password
`sudo koha-passwd bikelibrary`
# Username for temp: koha_bikelibrary
# Password for temp: randompasswordtext
# Press enter to clear the screen...

## Adjust firewall to allow access to public 
sudo ufw allow 80
sudo ufw allow 443

### Create A Records in crazy domains for:
#bikelibrary.website.com
#bikelibrary-intra.website.com

# Adding https to domainname (after all other instructions) 
sudo apt install certbot python3-certbot-apache -y

# Create SSL certificates for just admin mode (will add the regular one later)
sudo certbot --apache -d bikelibrary-intra.website.com
### Will request information
#Add email address
my_email_address@email.com
#Agree to terms and conditions
y
#Dont share email address
n
###

## Configure instance 
### Open web broswer
http://bikelibrary-intra.website.com

NOTE: Can watch video to configure
https://www.youtube.com/watch?app=desktop&v=RLvdhERXfAM

### Choose language
en => "Continue to next step"
### Check Perl dependencies
"Continue to next step"
### Database settings
"Continue to next step"
"Continue to next step"
### Set up database
"Continue to next step"
"Continue to next step"
### Install basic configuration settings
"Continue to next step"
   #### Select your MARC flavor
   Marc21
   #### Selecting default settings
   some basic default authorised values for library locations
   CSV profiles
   #Others: MARC code list
   some basic currencies with USA dollar
   #Useful patron attribute types
   Sample patron types and categories
   Sample label and patron card data
   A set of default item types
   "Import"
   "Set up some of Koha's basic requirements"
   "Begin the onboarding process"
## Installation
### Create a library
library code: BIKE
Name: Bike Library
### Create Koha administrator patron
Surname: 
First name: 
Card number: 001
Library: Bike Library
Patron category: Staff
  #Admin login
  Username: set_username
  Password: set_password
  Confirm password: 
#Create a new item type
Left as default

# Once installed expand certificates to cover both pages
`sudo certbot --apache --expand -d bikelibrary-intra.website.com -d bikelibrary.website.com`
