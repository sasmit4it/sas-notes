## C2 instance ip

each EC2 instance has two ip addresses. 1 is private ip, and other is public ip. Private ip is aws internal ip and it remains same though out the Ec2 instances life cycle.

Public ip is dynamically assigned to ec2 instance every time, the instance starts up. Whe you stop and start the instance, it gets a new ip address assigned.

This has one exception. When you choose to reboot the instance from settings, public ip remains the same.

## Elastic IP

Elastic ip is a static IP that can be assigned to EC2 instances. This ip will not change when instance is stopped and started.

Elastic ip is chargeable if

1. you provision it but do not associate it with any instance
    
2. you associate the ip with instance, but instance is in stopped state.
    
3. you reassign the ip to different instance more that 100 time.
    

## User data

user data is the script that runs when the ec2 instance starts up.

instance metadata service can be accessed from the instance console on url:

[http://169.254.169.254/latest/meta-data](http://169.254.169.254/latest/meta-data "http://169.254.169.254/latest/meta-data")

## EC2 imps

1. You cannot change instance type of a running instance. To change it’s type, you’ll have to stop the instance and then change type from actions-> instance settings
    

2. AWS provides option of instance protection. When enabled , to will not allow users to terminate the marked instance. This protection however applies only to user provided termination requests. It will not apply against OS shutdown, load balancer initiated termination etc.
    

3. You cant change AMI of existing EC2 instance. You have to terminate and relaunch a new instance.
    

4. You create EC2 instance from VM images available locally. You can use VM import tool.
    

5. You are not charged for stopped instance, however storage attached to that instance is still chargeable.
    

## AMI

Amazon machine images. These are predefined images used to launch new ec2 instances. EAMI contains OS and pre-defined software's bundled with that OS. There are tree types of AMIs.

1. defaults provided by aws
    
2. third party AMIs available on AMI marketplace.
    
3. custom AMIs created by you
    

Custom AMIs can be created from EC2 instances. By default when we create an AMI from EC2 instance, EC2 instance is stopped, snapshot created and instance is re-started again. How ever you can disable this restart from options, but this is not a recommended way.

AMI contains

1. root volume block storage(OS and apps)
    
2. block device mapping for non-root volumes
    

AMIs are stored in S3. **AMIs are region specific**. You can create AMI copy in different region, and using that copy you can launch instance in new region.