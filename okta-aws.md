Utilizing AWS cli is now handled by an internally packaged aws bin for the Okta AWS SSO implementation.

What you need:
Okta account
Assignment of AWS App in Okta
Terminal knowledge
AWS Cli install and knowledge
Okta AWS Cli Installation:
For macOS/Linux
Install awscli
You may use either of the 2 methods below. Use pip or brew to install awscli as either of these package managers will give you access to the correct executable:

PIP install method [Link:AWS Docs]:

my.user@ThisMac > curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py

#Then run the following:
my.user@ThisMac > python get-pip.py

my.user@ThisMac > pip install awscli --upgrade --user


Brew install method:

# Install brew if you do not have it:
my.user@ThisMac > /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

# Install the awscli via the brew package manager and installer:
my.user@ThisMac > brew install awscli




my.user@ThisMac > export PREFIX=/usr/local

my.user@ThisMac > PREFIX=~/.okta bash <(curl -fsSL https://raw.githubusercontent.com/oktadeveloper/okta-aws-cli-assume-role/master/bin/install.sh) -i

my.user@ThisMac > vim ~/.bash_profile 

#####
# Add these lines to your .bash_profile
#####

#OktaAWSCLI
if [[ -f "$HOME/.okta/bash_functions" ]]; then
    . "$HOME/.okta/bash_functions"
fi
if [[ -d "$HOME/.okta/bin" && ":$PATH:" != *":$HOME/.okta/bin:"* ]]; then
    PATH="$HOME/.okta/bin:$PATH"
fi
# Source your bash file:
my.user@ThisMac > . ~/.bash_profile


Add your information to the Okta config file:
Your config.properties should only include these lines:



my.user@ThisMac > vim ~/.okta/config.properties

#OktaAWSCLI
OKTA_ORG=aetion.okta.com
OKTA_AWS_APP_URL=https://aetion.okta.com/home/amazon_aws/0oa7bep0fFoVNB6Gc356/272
OKTA_USERNAME=firstname.lastname@aetion.com # Edit your Okta username/email


If you do not have a Java JDK installed you will need to install one:

If you have do not have it installed click more info and Download and install the Java JDK

Install via brew:



my.user@ThisMac > brew cask install oracle-jdk




### OUTPUT
==> Caveats
Installing oracle-jdk means you have AGREED to the license at
  https://www.oracle.com/technetwork/java/javase/terms/license/javase-license.html

==> Satisfying dependencies
==> Downloading https://download.oracle.com/otn-pub/java/jdk/11.0.2+7/f51449fcd52f4d52b93a989c5c56ed3c/jdk-11.0.2_osx-x64_bin.dmg
==> Downloading from https://download.oracle.com/otn-pub/java/jdk/11.0.2+7/f51449fcd52f4d52b93a989c5c56ed3c/jdk-11.0.2_osx-x64_bin.d
######################################################################## 100.0%
==> Verifying SHA-256 checksum for Cask 'oracle-jdk'.
==> Installing Cask oracle-jdk
==> Running installer for oracle-jdk; your password may be necessary.
==> Package installers may write to any location; options such as --appdir are ignored.
Password:
installer: Package name is JDK 11.0.2
installer: Installing at base path /
installer: The install was successful.
🍺  oracle-jdk was successfully installed!


Login to Okta via the cli:



my.user@ThisMac > okta-credential_process
Username: firstname.lastname@aetion.com
Password: ### ENTER YOUR OKTA PASSWORD ###


If you receive Exception: com.amazonaws.services.securitytoken.model.AWSSecurityTokenServiceException: Request ARN is invalid this may be due to incompatibility issue with okta-aws-cli-2.0.2.

Download and replace symlink to version https://github.com/oktadeveloper/okta-aws-cli-assume-role/releases/tag/v2.0.0

my.user@ThisMac > cd ~/.okta
my.user@ThisMac > mv ~/Downloads/okta-aws-cli-2.0.0.jar .
my.user@ThisMac > rm okta-aws-cli.jar
my.user@ThisMac > ln -s okta-aws-cli-2.0.0.jar okta-aws-cli.jar






### OUTPUT
Auto select role as only one is available : arn:aws:iam::179392497636:role/OktaAdminAccount
{"Version":1,"AccessKeyId":"ASIASTRFxxxxxR","SecretAccessKey":"ewmbRcJAajtdEz0zvbxxxxPH6o65KrDNRv2iv","SessionToken":"FQoGZXIvYXdzEPr//////////wEaDOLjn2hLrKUM45nCVyKIAkw3RmPuXSZ6siUfjTFtKtLcQHT8otxQcChh0WovL6CbyiZMAOWq6jVCib7KA0yZcdsX2b6z9hHKAPdyNcsgdWcN6Z8L90mGFXv8FVdVgCwXhffTA9I2xeV1aLRNEenbCz00XX56+ZrWCzJyFceOG/DRWFEbaV2qZYSo52tbN2uogJYtB/Iat00evFV/2FpuqVob34odUdjoB8N+kKk/QbTI+YNkx73B491To9+hqPjnRrJAgkEgvyyEdU253vBEB8QzIybN4IRI9I0JLzcjHxTcUNAisEa/sqROW1fzCE2Lrel+6x8E7KUruV/Wxva1gjdWWp8SnQsjmb2xGB4GQLArcE0CnktBkCib2oLiBQ==","Expiration":"2019-01-17T17:26:22.684514Z"}


Done! Now you can use okta-credential_process each time you want to utilize AWS Cli and generate a session token.



Okta AWS Cli Usage:
Confirm you have all available Okta commands on your terminal:

my.user@ThisMac > okta- # TAB here twice for autocomplete to see list of available commands
okta-aws                 okta-credential_process  okta-listroles           okta-sls


Get a temporary user accesskey, secret key, and token:

my.user@ThisMac > okta-credential_process 


If you have more than one role you can select them from a text menu as seen in this example:

my.user@ThisMac > okta-credential_process 

Please choose the role you would like to assume: 
Account: aetiondev (179392497636)
[ 1 ]: OktaAdminAccount
[ 2 ]: OktaAllAWSReadOnly
[ 3 ]: OktaReadOnlyS3
Selection: 1 # Your selection is done here# This is your output from your selection
{"Version":1,"AccessKeyId":"ASIASTRFAI7SBAM627NN","SecretAccessKey":"fg9Kt9JskskC1ZJ+ZNpE5drc8ctJfDz3vLPJpq/c","SessionToken":"FQoGZXIvYXdzELb//////////wEaDD0OozESyhVowApzYSKIAjIdznAntHIYAx5mfIvII4HBeELfLvAXqz9VbhsKXFF4VdMElM4LbwzwP4wI5T8+U9wsHYC+ZD4CZbOdXQ60Pfcq1hmCTzdHWwQIzDWcLng9OSS8BhMeK+dkC0zmGHRcjBWu3KCmRIvu+2ozjPA1WGfJqeraYV/rR+zvu9zNtkEZADs14uNaD/1rOGoZo9yZkLrF110cnEO/zfS70jGUhT0XnW/SXWsjBSHqPsUppg63F1D0QqlXUp65cdbxDaoQSc0f6989qdeMWUlBVNQTUgWvOWoehbqpTCyLrZqn8N2Av7ww9R7bmzwaLQqxfBNEZJ6M9dFMO+cqOlhCKcHRNA1GcHjpH6sXkyjA3fPhBQ==","Expiration":"2019-01-14T21:19:07.775722Z"}


To use aws cli you now perform all your actions with the prefix command "okta-aws" as seen in this example:
Previous AWS use:

my.user@ThisMac > aws iam list-users

### OUTPUT FROM COMMAND:
{
    "Users": [
        {
            "Path": "/",
            "UserName": "xxxxxx",
            "UserId": "xxxxx",
            "Arn": "arn:aws:iam::179392497636:user/xxx",
            "CreateDate": "2018-04-11T19:17:45Z",
            "PasswordLastUsed": "2019-01-14T16:29:26Z"
        },
        {
            "Path": "/",
            "UserName": "...",
            "UserId": "xxxxxx",
            "Arn": "arn:aws:iam::xxxx:user/xxx",
            "CreateDate": "2015-08-15T17:20:20Z",
            "PasswordLastUsed": "2015-09-10T11:43:42Z"
        },
...
        {
            "Path": "/",
            "UserName": "...",
            "UserId": "xxxxxx",
            "Arn": "arn:aws:iam::xxxx:user/xxx",
            "CreateDate": "2015-08-15T17:20:20Z",
            "PasswordLastUsed": "2015-01-12T10:12:11Z"
        }
...  
  ]
}


Current AWS use:

In order to use the command we assign the temporary credentials we created the profile name of "default"
Use:
Usage

Verify your setup with a simple command:

okta-aws default sts get-caller-identity
This will prompt for Okta credentials, log you into AWS, let you pick a role, and store a session profile called test for you.

Run the program again to see session resumption (you won't be asked for Okta credentials until the session expires):

okta-aws default sts get-caller-identity
NOTE: okta-aws is a function loaded from your shell profile, not a typical program or command stored in a file.


Example:




my.user@ThisMac > okta-aws default iam list-users

### OUTPUT FROM COMMAND:
{
    "Users": [
        {
            "Path": "/",
            "UserName": "xxxxxx",
            "UserId": "xxxxx",
            "Arn": "arn:aws:iam::179392497636:user/xxx",
            "CreateDate": "2018-04-11T19:17:45Z",
            "PasswordLastUsed": "2019-01-14T16:29:26Z"
        },
        {
            "Path": "/",
            "UserName": "...",
            "UserId": "xxxxxx",
            "Arn": "arn:aws:iam::xxxx:user/xxx",
            "CreateDate": "2015-08-15T17:20:20Z",
            "PasswordLastUsed": "2015-09-10T11:43:42Z"
        },
...
        {
            "Path": "/",
            "UserName": "...",
            "UserId": "xxxxxx",
            "Arn": "arn:aws:iam::xxxx:user/xxx",
            "CreateDate": "2015-08-15T17:20:20Z",
            "PasswordLastUsed": "2015-01-12T10:12:11Z"
        }
...  
  ]
}


To get your info run:


my.user@ThisMac > okta-aws default sts get-caller-identity{
"UserId": "USER_ID:USER",
"Account": "#####",
"Arn": "arn:aws:sts::#####:assumed-role/ROLE_NAME/USER"
}





References:

AWS Pip Docs
Brew install
AWS User Federation with Okta – Part 1: Console Access
AWS User Federation with Okta – Part 2: Multi-Factor Authentication
AWS User Federation with Okta – Part 3: CLI Access
Integrating the Amazon Web Services Command Line Interface Using Okta
Okta AWS CLI Assume Role tool
Revoking IAM Role Temporary Security Credentials
How to Easily Identify Your Federated Users by Using AWS CloudTrail
How do I use an MFA token to authenticate access to my AWS resources through the AWS CLI?
Github Okta cli repo: okta-aws-cli-assume-role
