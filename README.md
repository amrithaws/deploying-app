# deploying-app
The main concept is we will hace servers which will be deployed on cloud which will be private.
But the url of the app it will public which we put it in public.
First create a vpc with CIDR of 10.1.0.0/16
Create public subnet with range of 10.1.1.0/24 with name of public_netlix-2a
Create one more public subnet with range of 10.1.2.0/24 with name of public_netlix-2b
Create private subnet with range of 10.1.3.0/24 with name of private_netflic-2a
Create private subnet with range of 10.1.4.0/24 with name of private_netflic-2b
Create a IGW and attach to the netflic VPC
Now we have to create the rout table where Our vpc would have been created one route table name it as Internet-RT-Netflx.
Now create another routtable for our VPC with name noninternet-RT-Netflix.
now associate the subnets the rout tables.
for internet-RT-neflix associate public_netflix-2a and public_netflix-2b along with routes add route through IGW
for noninternet-RT-netflix associate private_netflix-2a and private_netflix-2b but no routs to added for this as it is noninternet route table.
Make sure to make the public_netflix-2a and public_nrtflix-2b both are enabled as auto assign public address under actions tab by selecting them.
Now lunch one server with ubuntu distribution.
under security group of the server edit that to all trafic but keep ki my ip.
Now ssh into the ubuntu saerver and update the server with cmd sudo apt-get update.
Now install web server called apache2 cmd for that is sudo apt-get install apache2
Next is website architecture
Now go to the cd /var/www/html where we ls we will get file as index.html
so if change index.html change the page will change in front end, so for now lets delete the index.html page by sudo rm index.html
Now create your own index.html page and write below code
<html>
  <body>
    <h1> Welcome to the home page</h1>
  </body>
  </html>
  save and exit :wq!
  Now create the image or AMI of the image which we launched earlier.
  Now againg go the index.html and modify the index.html as below
  <html>
  <body>
    <h1> Welcome to the streaming page</h1>
  </body>
  </html>
  save and exit :wq!
  Now create the image or AMI of the image which we launched earlier.
  We have to launch the 2 servers through the AMI's or image which we created 
  as we have launched the 2 servers with the 2 images now inorder to talk to private subnets we need our pem file
  to copy pem file from desktop to ec2 instance we can do it via Winscp 
  now ssh into private server which is netflix-homepage through >>>> ssh -i jenkins.pem ubuntu@10.1.3.38>>>> which is your netflix home page private address.
  now check if apache2 is active or not as we have configured befor to check command is sudo systemctl status apache2
  come out of the private server via logout command
  come to the public subnet public server
  againg ssh into private server which is netflix-streaming through >>>> ssh -i jenkins.pem ubuntu@10.1.4.85>>>> which is your netflix home page private address.
  now check if apache2 is active or not as we have configured befor. tocheck command is sudo systemctl status apache2
  
  Lets create the load balancer here
  reason for creating the application load balancer is it provides the path based routing.
  here while creating the load balancer we have to keep in mind that it will allow only HTTP protocall only
  after that select to AZ of load balancer, inorder to connect to private servers the az's should be public subnets because its internet facing ALB.
  create two target groups one is metflix-homepage and netflix-streaming and register the private instances which we created netflix-homepage for netflix-homepage     target and netflix-streaming for netflix-streamimg target.
  Now we can specify the rules for the load balancer.
  under add rules as if url/home then forward to welcome to home page (means netflix-homepage target group)
  and if url/watch then forward to the welcome to the streaming page (means netflix-streaming traget group)
  In order to redirected to the /home and /watch url there is small modification required in server.
  ssh into public server first and ssh to netflix-steaming server and uder
  cd /var/html create folder
  sudo mkdir watch
  and paste the file present in cd /var/www/htlm which is index.html
  commd for pasting the file sudo cp ./index.htlm ./watch
  perform the same for the netflix-homepage server as well
  
  NOTE: WHILE SPECOFYING THE RULE IN THE LOAD BALNCER MAKE SURE TO PUT THE * such AS
  /home* and /watch*
  
  
  LAST STEP is
  Go to the freenom website basically it is website which provide the free domain names 
  after that go to route53 create a hosted zone and give the domain name which is created by you and enter name servers which you bought from site in freenom.
  after create a record and select alias in aws and select load balancer and save .
  and create and another record and with alias and give www on name and select netflix load balancer.
  by which whenever you hit that domain name your website will open up.
  
  
 
