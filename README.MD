# BinaryAlert: Serverless, Real-Time & Retroactive Malware Detection

[![Build Status](https://travis-ci.org/airbnb/binaryalert.svg?branch=master)](https://travis-ci.org/airbnb/binaryalert) [![Coverage Status](https://coveralls.io/repos/github/airbnb/binaryalert/badge.svg?branch=master)](https://coveralls.io/github/airbnb/binaryalert?branch=master) [![Documentation Status](https://readthedocs.org/projects/binaryalert/badge/?version=latest)](http://www.binaryalert.io/?badge=latest) [![Slack Channel](https://binaryalert.herokuapp.com/badge.svg)](http://binaryalert.herokuapp.com)


![BinaryAlert Logo](docs/images/logo.png)

BinaryAlert is an open-source serverless AWS pipeline where any file
uploaded to an S3 bucket is immediately scanned with a configurable set
of [YARA](https://virustotal.github.io/yara/) rules. An alert will fire
as soon as any match is found, giving an incident response team the
ability to quickly contain the threat before it spreads.

Read the documentation at [binaryalert.io](https://binaryalert.io)!

## Links

-   [Announcement
    Post](https://medium.com/airbnb-engineering/binaryalert-real-time-serverless-malware-detection-ca44370c1b90)
-   [Documentation](https://binaryalert.io)
-   [Slack](https://binaryalert.herokuapp.com) (unofficial)

## How to Deploy (Linux)
### Install Python 3.8
For Ubuntu 18.04 LTS follow the commands below
```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.8
python3.8 --version
```

### Install Terraform
For example, to install terraform 0.12.31 use the following commands
```
git clone https://github.com/tfutils/tfenv.git ~/.tfenv
export PATH="$HOME/.tfenv/bin:$PATH"
tfenv install 0.12.31
tfenv use 0.12.31
terraform --version
```

####  To ensure that you have the necessary development files for Python 3.8
Run the command `sudo apt install python3.8-dev`

###  Install the OpenSSL development library
Ubuntu `sudo apt-get install libssl-dev`<br>
Amazon Linux `sudo yum install openssl-devel`

### Clone the latest python3.8 release of BinaryAlert:
`git clone --branch migrate_to_python3.8 https://github.com/airbnb/binaryalert-test`

### Create and activate a virtual environment
```
cd binaryalert
python3.8 -m venv venv
source venv/bin/activate
```

### Install the BinaryAlert requirements
```
python3.8 -m pip install --upgrade pip
pip3 install -r requirements.txt
```

### Run unit tests to make sure everything is installed correctly
`./manage.py unit_test`

### Set your AWS credentials using any method supported by Terraform. For example, using the AWS CLI
`pip3 install --no-cache-dir awscli`<br>
`aws configure`

### Configure BinaryAlert settings
`./manage.py configure`
```
AWS Region (us-east-2):
Unique name prefix, e.g. "company_team": your_unique_prefix
Enable the CarbonBlack downloader? (no):
```

### Deploy the applications
`./manage.py deploy`
<i>Terraform will generate a plan and request approval before applying</i>

The above command is equivalent to the following 3 operations executed in sequence:
```
./manage.py unit_test  # Unit tests ensure YARA rules compile correctly
./manage.py build      # Build the Lambda ".zip" deployment package(s)
./manage.py apply      # Update the infrastructure and deploy Lambda functions
```

### BinaryAlert is live! Test it by uploading a harmless EICAR test string
`./manage.py live_test`

### Debug and check terraform
```
cd terraform/
terraform plan  # Show pending changes
terraform show  # Print the current state of the infrastructure
```

### To teardown all of the BinaryAlert infrastructure
`./manage.py destroy`