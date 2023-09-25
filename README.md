# AWS Elastic Block Storage (EBS) use for MySQL data persistence on two separate EC2 instances
Here created an EC2 instance on AWS, attached a 30GB EBS volume to it, and deployed a MySQL Docker instance on that EC2 instance. After creating and populating a database on this instance, detached the EBS volume and attached it to a second EC2 instance. Upon checking the data on the second instance, confirmed the successful migration of the MySQL database. This demonstrates how to move data between EC2 instances using EBS volumes while maintaining data integrity and availability.

<img width="474" alt="AWS_EBS_MySQL2" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/b8f36817-6c1f-4726-ac4c-062ab9e0ecb5">

## Step 1: Creating 2 EC2 instances on AWS
<img width="960" alt="1" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/2158b469-1818-4b74-8f88-977ac29c75c3">

<img width="958" alt="2" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/a583803d-a1bb-4ec9-89f7-95818f2e9d16">

<img width="960" alt="3" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/f99c03e7-5d4b-4f7b-be08-cc8762860217">

<img width="960" alt="4" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/5759a77c-482a-4b33-8e19-720f35553689">

<img width="956" alt="5" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/17d31f22-1ad2-4356-b860-2113708e5bcf">

<img width="960" alt="6" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/518c0dc8-8f1a-49b0-a5b9-77a4653c6186">

Here Two EC2 instances has been created and running
<img width="960" alt="7" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/3f2d9a52-851f-4f33-af5f-0b4055893d6a">

## Step 2: Create an EBS storage
<img width="960" alt="8" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/0d671d06-4c1c-49e9-b24e-038493b667b1">

<img width="960" alt="9" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/b207c422-e7a9-494c-ae34-b18079c68d03">

<img width="960" alt="10" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/faaa8560-aba1-414c-aabe-c5ff2529fb98">


## Step 3: Attach the EBS to 1st MySQL Ubuntu OS
<img width="960" alt="11" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/9a33e598-9173-4790-9059-6b70dbd12343">

<img width="960" alt="12" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/2b16aaa2-bf2a-4310-a41c-a3148b843125">

<img width="960" alt="13" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/119de47b-b182-45b4-af6d-7d4f38e44a24">


## Step 4: Connect to 1st instance and create mount point on EBS
<img width="960" alt="14" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/4368dcc1-2327-47e7-9578-518c65b5dd99">

```bash
sudo apt update
sudo apt install docker.io docker-compose –y

git clone https://github.com/MdAhosanHabib/mysql-db-data-persistence.git
cd mysql-db-data-persistence

mkdir data
sudo lsblk
sudo file -s /dev/xvdf
sudo mkfs -t xfs /dev/xvdf
sudo mount /dev/xvdf /home/ubuntu/mysql-db-data-persistence/data
sudo docker-compose up -d
```
<img width="960" alt="15" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/8aae5a28-3a9e-444c-9b06-56a0d9444962">

<img width="960" alt="16" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/40e168f0-cb5a-4b2b-8b95-2d852e5a5757">

<img width="960" alt="17" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/67385f08-ed44-4e5e-80d5-04b09e046303">


## Step 5: Create test database on 1st instance
```bash
sudo docker exec -it db bash
mysql -root -p a
show databases;
create database ahosan;
show databases;
```

<img width="960" alt="18" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/f998c5bb-cc95-40bf-baa4-d1ea0f802bb4">


## Step 6: Detach the EBS and attach it to 2nd instance
<img width="960" alt="19" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/73ed88be-cffe-46f2-8da9-747edb822250">

<img width="960" alt="20" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/2b5c9c59-006c-4d4e-a3fe-2b18180df4ee">

<img width="960" alt="21" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/be62270a-3c37-4c7c-855c-841da84459c6">


## Step 7: Create mount point on EBS and set MySQL to retrieve the created Database
```bash
sudo apt update
sudo apt install docker.io docker-compose –y

git clone https://github.com/MdAhosanHabib/mysql-db-data-persistence.git
cd mysql-db-data-persistence

mkdir data
sudo lsblk
sudo mount /dev/xvdf /home/ubuntu/mysql-db-data-persistence/data

sudo docker-compose up -d
sudo docker exec -it db bash
mysql -root -p a
show databases;
```

<img width="960" alt="22" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/962de1a0-5ae4-429a-8be0-78db18462a3d">

<img width="960" alt="23" src="https://github.com/MdAhosanHabib/mysql-db-data-persistence/assets/43145662/c66ac01f-512d-4d67-9c71-4b6769762d98">

#### Data are same as Instance 1, Thanks from Ahosan Habib.

