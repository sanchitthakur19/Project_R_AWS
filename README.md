# Ruby on Rails PhotoSite Application and Deploying it on Amazon Web Services using Docker

## Section 1 and 2

### Introduction
This project demonstrates the work done using Ruby on Rails development and its deployment using Docker, DockerHub and Amazon Web Services using AWS S3 storage bucket and AWS EC2 instance creating and deploying application onit.
This is a Photo-sharing application made using Ruby on Rails. 
A user can view photos and comments by clicking on the name of a user.The application has a comment section that has its comments stored in an SQLite database. The images used for this application are stored in the S3 bucket. Docker image for this project is created and stored on Docker Hub. This application is deployed and runs on an AWS EC2 instance using a docker image from Docker Hub.
EC2 instances support Elastic IP address so that EC2 instance start and stop instance doesn't change the public IP address for access to the Photo-sharing application.
The EC2 instance has an image copy saved so that when the EC2 instance is under heavy load or has a failure copy, this EC2 instance can be launched for load balancing without the need for Photo-sharing application deployment again and quickly.

Youtube: https://youtu.be/sZrrzpZJA9U

Application Link: http://52.205.126.107:3000/

Github: https://github.com/sanchitthakur19/Project_R_AWS/

Github Wiki: https://github.com/sanchitthakur19/Project_R_AWS/wiki

### Creating Ruby on Rails Photo-Sharing Application

#### Steps to create a Ruby on Rails Application.

* Create a new Ruby on Rails application project.
  New Project -> Rails -> application -> add Ruby SDK and Rails version. For database select sqlite3. Click Create
* Create two Controllers for user and photo
* Go to Tools -> Run Rails Generator -> select rails g controller->Give appropriate name to the model
* Create three models for user, photo, comment
  Go to Tools -> Run Rails Generator -> select rails g model-> Give appropriate name to the model along with columns and data type  for creating a database
* Go to
  db -> migrate 
You will see files generated for the user, photo and comment. Add the columns you want to add in the database and specify their data type. 
* Create a new data migration file load_data or you can load the data manually through the database console by creating table
* Run the migration file by the following command RubyMine IDE: 
  rake db:migrate
* Go to views and create index.html.erb file for photo and user 
* Update routes.rb file to serve user/index and photo/index/:id pages
##### Running Photo-Sharing Application locally using Ruby Mine IDE
1. Run this project using Ruby Mine IDE. This will start server in listening mode.
![screenshot image](screenshot/screenshot1.PNG)
2. Open "http://localhost:3000/user/index" PhotoSite URL in browser. This shows PhotoSite app's HOME page.
Click on index #4 and it will open new page. This will shows images, comments posted with date & time.
![screenshot image](screenshot/screenshot2.PNG)
![screenshot image](screenshot/screenshot3.PNG)
![screenshot image](screenshot/screenshot4.PNG)

##### Demonstrating Application deployed successfully on Docker
Docker is used to create image of the container of the application. It helps in reducing the processing power and the efforts to install dependencies seperately for each application. The docker image can be pulled by anyone and deploy it in no time and this help is easy deployment of application on AWS.
To create the docker image we need to add Dockerfile and a docker-compose.yml file in the root repository of Ruby on Rails Application. Follwing this steo we will have to create a docker hub account if one does't have it and create a repository in it.
After that we create the docker image which is used for deploying on AWS EC2 instance.
######## Creating Docker file in application
1. Add the docker file in the Ruby Mine IDE project. This contains the ruby version alongs with other dependencies with the installation instruction for creating docker image.
![screenshot image](screenshot/screenshot5.PNG)
2. Add docker-compose.yml file in the project. This contains version and rails server port information.
![screenshot image](screenshot/screenshot6.PNG)
######## Creating Docker Hub repository
1. After logging in with docker hub account, click on repository and enter the repository name and the description for the repository.
![screenshot image](screenshot/screenshot7.PNG)
2. After creating the repository we will be able to push the docker image onto it and it will show the command for the same.
![screenshot image](screenshot/screenshot8.PNG)
######## Building Docker Image in Ruby Mine IDE
1. Go to the application folder directory using the terminal where the dockerfile is present and run the command
  docker build -t amhatre2/group5:project_r_aws_web .
 ![screenshot image](screenshot/screenshot9.PNG)
  this has the name of the repository as well as a tag as flag 
2. After creating the docker image run it on the local system using the command
  docker run amhatre2/group5:project_r_aws_web
![screenshot image](screenshot/screenshot10.PNG)
3. After confirming that the docker is running we can push it to our repository using command after loging in to docker hub 
  docker push amhatre2/group5:project_r_aws_web
![screenshot image](screenshot/screenshot11.PNG)
4. We build docker-commpose file
docker-compose build
![screenshot image](screenshot/screenshot12.PNG)
5. After creating we run the docker compose file and create and migrate database
docker-compose run web rails db:create db:migrate
![screenshot image](screenshot/screenshot13.PNG)
6. After that use the following command to aggregate the output.
docker-compose up
![screenshot image]screenshot/(screenshot14.PNG)
Application running on local host on docker
![screenshot image](screenshot/screenshot15.PNG)
![screenshot image](screenshot/screenshot16.PNG)
![screenshot image](screenshot/screenshot17.PNG)

### Amazon Web Services EC2 Compute
We'll start creating an instance in AWS

We have selected the first option
![screenshot image](screenshot/Picture1.png)
Then We have chosen t2 micro.
![screenshot image](screenshot/Picture2.png)
We have selected default storage, configurations, and tags
We have configured the security group as follows
1. Add rule where 
2. Type is Custom TCP rule
3. Port is 3000
4. Source is anywhere
![screenshot image](screenshot/Picture3.png)
Now we will setup a new key pair to access our server and store the private key somewhere safely on our local computer.
![screenshot image](screenshot/Picture4.png)
Now we will allocate on elastic IP 
And associate it to the server
![screenshot image](screenshot/Picture5.png)
The Elastic IP associated to the server 
![screenshot image](screenshot/Picture6.png)
### Create an S3 bucket on Amazon Web Services
1. Search for S3 bucket
2. Go to Create
3. And enter the bucket’s name
![screenshot image](screenshot/Picture7.png)
4. Uncheck the block all public access so that you could access your bucket from public IP
![screenshot image](screenshot/Picture8.png)
5. And click submit
Your bucket is created successfully
![screenshot image](screenshot/Picture9.png)
6. Now upload photos to the bucket through add files option
![screenshot image](screenshot/Picture10.png)
![screenshot image](screenshot/Picture11.png)
7. Now set up permissions for the photos uploaded in the bucket
Go to the permissions tab -> Click edit
![screenshot image](screenshot/Picture12.png)
8. Now setup the permissions as follows
The type of policy will be S3 bucket policy
Principal will be *
Action will be Get Object and Put Object.
![screenshot image](screenshot/Picture13.png)
9. Copy and paste the newly generated policy to the edit bucket policy tab
And hit save changes.
![screenshot image](screenshot/Picture14.png)
10. After that, we will connect putty and run the commands to deploy docker on putty

Sudo yum update -y
![screenshot image](screenshot/Picture15.png)

11. Then run 
sudo yum install docker -y
![screenshot image](screenshot/Picture16.PNG)

12. Then run 
sudo service docker start
![screenshot image](screenshot/Picture17.PNG)

13. Then run 
sudo docker run -it -p 3000:3000 amhatre2/group5:project_r_aws_web
![screenshot image](screenshot/Picture18.PNG)
######## Accessing the application deployed on Amazon AWS using public ip
![screenshot image](screenshot/Picture19.PNG)
![screenshot image](screenshot/Picture20.PNG)
![screenshot image](screenshot/Picture21.PNG)

### SECTION 3) This application is working properly end to end. No open issue is present.

### SECTION 4)  YouTube URL
https://youtu.be/sZrrzpZJA9U


### Section 5)
what happens when an instance stops running?

If an instance stops running, any data stored in the instance store volumes of the host computer or the RAM of the host computer is gone but the data on Amazon EBS Volumes will remain attached to the instance.


### Section 6)
what happens when you reboot an instance and what can you do?

When you reboot an instance it retains its private IPv4 addresses and any IPv6 addresses when stopped and started. But public IPv4 address is released and a new one is assigned when you start your instance again. 
The instance retains its associated Elastic IP addresses.  With EC2-Classic, an Elastic IP address is dissociated from your instance when you stop it. An Elastic IP address is a property of a network interface. You associate an Elastic IP address with an instance by updating the network interface attached to the instance.

