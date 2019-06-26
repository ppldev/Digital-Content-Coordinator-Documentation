# Amazon AWS
---

We use Amazon Web Services for hosting for ProvLibDigital.org.  In order to connect to our AWS EC2 instance using an SFTP client or using SSH you need a [.PEM File](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html). That file needs to be created in the EC2 admin.  The PEM File I use is located on the DCC iMac at `/Users/dcc/Documents/AWS/ISLE-pem/ISLEIslandoraKey.pem`.  This file can be used on more then one machine.

**To connect to EC2 via an SFTP client**

1. Add the Host as found in the ec2 admin.
  1. For PLD the host is **ec2-18-217-10-41.us-east-2.compute.amazonaws.com**
2. For Logon Type select Key file
  1. User = ubuntu
  2. Key file: /path/to/the/PEM/file.PEM

**To connect to EC2 via SSH**

1. Open Terminal or iTerm
2. Navigate to the directory that contains the PEM file
2. From that directory enter this command `ssh -i [pem-file-name].pem [user]@[aws-ec2-url]`
  *
