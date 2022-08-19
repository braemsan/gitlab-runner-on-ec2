# gitlab-runner-on-ec2
Setting up Gitlab Runner on AWS Amazon Linux EC2

## Part 1: Create Amazon Linux EC2 in AWS
 Go to your AWS account
 
 Go to EC2 instance , click Launch instance
 
 Enter the name for the instance
 Under AMI, select Amazon Linux 2
 Instance type, t2.micro
 Create a new key pair
 Create security group, allow SSH traffic from anywhere
 Under storage, select gp2, click advanced, under Delete on termination, select yes
 Check the summary to confirm
 Click Launch instance

## Part 2: ssh to EC2 instance
 Open terminal
 Locate your private key file and cd to it
 Change the permission of the key by running this command:
  `chmod 400 <private_key.pem>`
 Connect to your instance using its Public DNS:
  `ec2-<public_ip>.ap-southeast-1.compute.amazonaws.com`
 Example:
  `ssh -i "<private_key.pem>" ec2-user@ec2-public_ip.ap-southeast-1.compute.amazonaws.com`
 
## Part 3: Installing GitLab Runner
 Once ssh to EC2 done, install by running these commands
  `sudo yum update`
  `curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh" | sudo bash`
  `sudo yum install gitlab-runner`
 
## Part 4: Register a runner
 Before registering, you need to obtain a few things from GitLab:
 GitLab Instance URL and Registration Token
 To obtain these, Log In to your GitLab
 Select the project, go to Settings > CI/CD > Runner > Expand
 Back to your terminal (EC2), run
  `sudo gitlab-runner register`
 Enter your GitLab instance URL (also known as the gitlab-ci coordinator URL).
 Enter the token you obtained to register the runner.
 Enter a description for the runner. You can change this value later in the GitLab user interface.
 Enter the tags associated with the runner, separated by commas. You can change this value later in the GitLab user interface.
 Enter any optional maintenance note for the runner.
 Provide the runner executor. For most use cases, enter docker.
 If you entered docker as your executor, you are asked for the default image to be used for projects that do not define one in .gitlab-ci.yml.

## Part 5: Confirm that GitLab Runner is running
 run these commands:
  `sudo gitlab-runner --version`
  `sudo gitlab-runner status`
  
Congratulations, you have successfully set up GitLab Runner on EC2 Instance
