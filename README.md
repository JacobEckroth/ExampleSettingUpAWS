# ExampleSettingUpAWS

Please watch [this youtube video](https://youtu.be/DHrZVC6NdkU) for a more thorough walkthrough.

## Domain Name Stuff

1. Get a domain name. You can buy them from many places, just google "buy a domain name".
2. Wherever your domain name is being hosted, whether it be GoDaddy, Google Domains, SquareSpace or otherwise, find where you can set the custom name servers. We'll come back to that later.

## AWS
1. Create an [AWS account](https://aws.amazon.com/)
2. Go to the EC2 dashboard by searching "EC2 in the top bar"
3. Create a new EC2 instance, it should be the first option: amazon 2 AMI SSD Volume Type (Make sure it’s free tier eligible)
4. Select t2.micro as the instance type. Ignore the warning
5. Press launch, and download the key pair. Store your private key in a safe location.

## SSHing in to your Instance
1. Download [MobaXterm](https://mobaxterm.mobatek.net/) (or some other SSH client) and start a new session.
2. Username: ec2-user
3. Remote Host: Whatever it says under *Public IPv4 DNS* on your instance on AWS
4. Click Advanced SSH and add your key under *use a private key*
5. You should now be successfully logged into your instance.

## Editing The Security Group
1. Go back to your insatnce on AWS and go to the security tab
2. Click the security group running (there should be only one)
3. Click "Edit Inbound Rules"
4. Add A new rule with type "HTTP" and source "Anywhere"

## Installing NGINX
1. To install NGINX on your EC2 instance type `sudo yum amazon-linux-extras install nginx1`
2. To start NGINX type `sudo systemctl start nginx`
3. If you want to check the status of the command type `sudo systemctl status nginx`

Now we can successfully visit our *Public IPv4 DNS* from earlier, and see the default NGINX page. The address should look something like this: ec2-35-83-168-38.us-west-2.compute.amazonaws.com

## Connecting our Domain Name
1. Create a new Route53 hosted zone on AWS for your domain.
2. Take all the name servers it gives you and put them in for custom name servers on your domain hoster.
3. Add an A record under the Route53 which redirects the url to the public IP for your EC2 instance, which should be under the **Public IPv4 address** listing on your instance dashboard.

Now you should be able to visit your domain name and see the default NGINX HTML page.

## Finishing Up with NGINX and Express
1. On your instance do `cd /etc/nginx/`
2. Remove the current nginx.conf file by doing `sudo rm -f nginx.conf`
3. Create a new nginx.conf file and copy the contents from the file from this repository into it. 
4. Run `sudo systemctl restart nginx` to reload the config file.
5. Create a folder anywhere, this will be where our server is located.
6. To install node, follow [these directions](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)
7. Then run `npm init` and press enter to skip through all the settings
8. Run `node install express` to install express
9. Create a file called server.js and copy the contents of the server.js file in this directory into it.
10. Run node server.js and you're done! Your "webpage" should now be accessible by going to your domain name on a browser.
