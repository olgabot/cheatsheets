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

The end of the build should look like this:

```
==> ubuntu16-ami: Stopping the source instance...
    ubuntu16-ami: Stopping instance, attempt 1
==> ubuntu16-ami: Waiting for the instance to stop...
==> ubuntu16-ami: Creating the AMI: -ubuntu-2017-10-24T15-32-56Z
    ubuntu16-ami: AMI: ami-6769a51f
==> ubuntu16-ami: Waiting for AMI to become ready...
==> ubuntu16-ami: Modifying attributes on AMI (ami-6769a51f)...
    ubuntu16-ami: Modifying: description
==> ubuntu16-ami: Modifying attributes on snapshot (snap-020f596c5669b82e8)...
==> ubuntu16-ami: Terminating the source AWS instance...
==> ubuntu16-ami: Cleaning up any extra volumes...
==> ubuntu16-ami: Deleting temporary security group...
==> ubuntu16-ami: Deleting temporary keypair...
Build 'ubuntu16-ami' finished.

==> Builds finished. The artifacts of successful builds are:
--> ubuntu16-ami: AMIs were created:
us-west-2: ami-6769a51f
```

## Using `aegea` to launch and manage Amazon machine instances (AMIs) from the command line

### Which machine should I use?

A really useful website for filtering and searching machine types is http://ec2instances.info/ for "Easy Amazon EC2 Instance Comparison."

### Launch an instance from the command line using `aegea`.

This is how to launch a large instance to only run for a few hours on AWS. This
is using the AMI image id `ami-6769a51f`, which is the Ubuntu + Anaconda 5.0.0
image I created with Packer above.

```
aegea launch --instance-type i3.4xlarge --duration-hours 2  --ami ami-6769a51f olgabot-anaconda-loom
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
