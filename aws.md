# Amazon Web Services

## Create a `packer` image

[`packer`](https://www.packer.io/) is a command line program to build machine
packages. To install it on Mac OS, use [`homebrew`](https://brew.sh/):

```
brew install packer
```

### Example `packer` file with Anaconda

Here's an example packer file called `anaconda-5.0.0.json` that installs version 5.0.0 of [Anaconda](https://www.anaconda.com/download/#linux).

```
{
  "min_packer_version": "1.0.0",
  "variables": {
    "aws_region": "us-west-2"
  },
  "builders": [{
    "name": "ubuntu16-ami",
    "ami_name": "{{user `user`}}-ubuntu-{{isotime | clean_ami_name}}",
    "ami_description": "An Ubuntu 16.04 AMI with Anaconda 5.0.0 (Release Date: September 26, 2017)",
    "instance_type": "t2.micro",
    "region": "{{user `aws_region`}}",
    "type": "amazon-ebs",
    "source_ami_filter": {
      "filters": {
      "virtualization-type": "hvm",
      "name": "ubuntu/images/*ubuntu-xenial-16.04-amd64-server-*",
      "root-device-type": "ebs"
      },
      "owners": ["099720109477"],
      "most_recent": true
    },
    "ssh_username": "ubuntu"
  }],
  "provisioners": [{
    "type": "shell",
    "inline": [
      "sleep 30",
      "whoami",
      "pwd",
      "wget https://repo.continuum.io/archive/Anaconda3-5.0.0.1-Linux-x86_64.sh -O anaconda.sh",
      "bash anaconda.sh -b -p $HOME/anaconda",
      "export PATH=$HOME/anaconda/bin:$PATH",
      "hash -r",
      "conda config --set always_yes yes --set changeps1 no",
      "conda update -q conda",
      "conda info -a"
    ]
  }]
}
```

### Build the image

```
paceker build anaconda-5.0.0.json
```

## Using `aegea` to launch and manage Amazon machine instances (AMIs) from the command line
### Launch an instance from the command line using `aegea`

```
aegea launch --instance-type i3.4xlarge --duration-hours 4  --ami ami-cd0f5cb6 olgabot-czirna1
```

### Log in to that instance
```
aegea ssh ubuntu@olgabot-czirna1
```

### Find your instances

Search instances you can see for ones that start with your username, e.g. for
my username `olgabot`:

```
aegea ls -t 'Name=olgabot*'
```
