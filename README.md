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
