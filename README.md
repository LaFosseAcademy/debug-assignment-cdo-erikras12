# Debug Assignment CDO

## Tour-de-France API
This API has CRUD functionality as it manages and stores information popular cycling teams that attend the tour-de-france. It allows users to search for specific teams/riders and allows the user to filter riders based on the type of rider they are. This appliation was hosted on a ec2 instance by running a docker-compose file on the instance via ansible and the respective docker images.

## How to Run

### Docker images
Firstly the user must navigate to the tour-de-france-db directory and it is here were the user must run the following commands:

``` docker build -t <docker-username>/tour-de-france-db:0.0.1.RELEASE . ```

Followed by:

``` docker run --name tour-de-france-db <docker-username>/tour-de-france-db:0.0.1.RELEASE ```

To check wether the db is running do the following:

``` docker exec -it tour-de-france-db bash ```
``` -U postgres -d tour-de-france ```
``` \dt ```

The user should now be able to see the two tables. (Type in exit to navigate out of that environment)

The user should now push the image to the public registry to avoid having to login into docker at a later stage:

``` docker push <docker-username>/tour-de-france-db:0.0.1.RELEASE ```

Now the user should navigate to the tour-de-france-mvc directory and from here just:

``` docker build -t <docker-username>/tour-de-france-mvc:0.0.1.RELEASE . ```

AND:

``` docker push <docker-username>/tour-de-france-db:0.0.1.RELEASE ```


## Terraform
The user should go to AWS and create a key pair for the us-east-1 region, calling the key 'default-ec2' and also storing it in '~/aws/aws_keys' as well as giving it permissions via:
``` chmod 400 default-ec2.pem ```

The user should install terraform and navigate to the terraform directory and run 'terraform init' also run 'terraform validate' to make sure there are no problems with files and then run:

``` terraform apply ```

Where prompted type in 'yes' and now you should have an instance running.

copy over you docker-compose,yml file into the instance via (make sure you in the same directory as you docker-compose.yml file):

``` scp -i ~/aws/aws_keys/default-ec2.pem docker-compose.yml ec2-user@<   your-instance-ip    >:/home/ec2-user ```

## Ansible
The user should install ansible (straightforward for mac/ubuntu users), for microsoft user it is recommended to use azure cli and copy over the default-ec2.pem file (making it sure to give it permissions like above) and pull the repo into the cli (ansible is already installed). The commands will be the same for both.

The user should now navigate to the ansible directory and from here should open up the ansible_hosts file and remove the ip address in there and replace it with the ip of their instance.

The user should then run the following commands:

``` ansible-playbook playbooks/docker-install.yml ```

AND THEN

``` ansible-playbook playbooks/docker-run.yml ```


## Access deployed application
To access the deployment th user should input their instance ip into the link below and then enter that into the url.

``` http://<instance-ip>:3000/tour-de-france ```

With that the user should have access to the api.



## Future Features

 - Pipeline: adding a pipeline that would enable changes to the repo to automactically change the deployed api would be a nice avenue to go down 
 - Run api on multiple servers: use loadbalancers to dynamically shift users to different servers based on traffic.

## Bugs 

No known bugs.
