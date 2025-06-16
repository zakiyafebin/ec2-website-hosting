#Hosting a Static Website with AWS EC2 Using Nginx
### Step 1 : Configure ec2 with a key pair pem and a security group.
>- Step 1.1 : Configure Security group
 ![image](https://github.com/user-attachments/assets/180c7c26-c9d5-4ac8-b1de-962e6a676981)
### Step 2 : Connect to the server using ssh
>-- Copy downloaded pem file to a folder . go to that folder. the
> - chmod 400 ec2-key-pair.pem
> - Run above command to change the autherization of pem file.
>  -Now copy pem file to ec2 instace home with the following command :
> - ssh-i <path_to_key_pair_file> ec2-user@<public_ip_from_dashbard>   => run this from the bash
> - Now our pem file will be in ec2 home
### step 3
>-  Elevating privileges and updating all the packages on the instance.
>-- sudo su

>--yum update -y
###Step 4 : Creating a static Nginx website
> - systemctl status nginx
> - systemctl start nginx
> - netstat -ntlp => Gives the current network connections and port activity
###Step 6: Update our Nginx web page to a static HTML
>- Using git clone cloned my static web app here it is my_app to ec2 home
>- From there
>- cp -r /home/ec2-user/my_app /usr/share/nginx/html

