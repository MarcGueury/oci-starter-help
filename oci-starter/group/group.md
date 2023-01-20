
# Groups

Groups allow to create "Common Resources" that you will share between several applications.
It can be:
- the Network (VCN/Subnet)
- a Kubernetes cluster (OKE)
- (optional) a database or a container database
- a bastion
- ...


But when you enable the *Advanced* option, you have 3 choices for Group: *None / New / Existing*

- A application is by default standalone. Using no group: *None*
- a new group with a first application: *New*
- or a new application to use with an existing group: *Existing*

## Architecture

### Compute

Here is the architecture of a group of applications sharing common resources with Compute

![Group Compute](images/architecture_common_compute.png =80%x*)

### Kubernetes

Here is the architecture of a group of applications sharing common resources with  Kubernetes

![Group Kubernetes](images/architecture_common_kubernetes.png =80%x*)

### Container-Instance

Here is the architecture of a group of applications sharing common resources with  Container Instance

![Group Container-instance](images/architecture_common_container_instance.png =80%x*)

### Function

Here is the architecture of a group of applications sharing common resources with Function

![Group Function](images/architecture_common_function.png =80%x*)

## Task 1: Group sharing Kubernetes and Database for several Micro-Services

### Group Common Resources and First application

- Click on *Advanced*
- Choose Group *New* 
    - You may click on the "..." to get more explanation
- Keep the *group name* "dev",    
- Choose Deployment *Kubernetes*
- Click *Cloud Shell*

![Group App1](images/starter-group-app1.png =80%x*)

- Copy the command 

```
curl "https://starter.wedoteam.io/app/zip?prefix=starter&group_name=dev&group_common=atp,oke&deploy=kubernetes&ui=html&language=java&database=atp" --output dev.zip
unzip dev.zip
cd dev
cat README.md
```

Notice that you have 2 directories:
- group_common for the common resources of the group (OKE, ATP)
- starter for the application

Configure the script:
- In Cloud Shell Or Cloud Editor, edit the file *group_common/env.sh*
- It is the exact same steps than for Kubernetes. You need to enter the TF\_VAR\_auth\_token (OCI Auth Token). Please refer lab 2 Kubernetes steps.

![Group App1 Env](images/starter-group-app1-env.png =80%x*)

- Run 

```
./build_group.sh
```

The build_group will first build group_common and then the application starter.

When done, check if the application works:

```
http://123.123.123.123/starter/
```

## Task 2: Create a second application 

This second application will reuse the same component than the previous one. (Network, Kubernetes, DB)
So, practically, it will just create new PODS in the cluster.

- Change the prefix to *starter2*
- Click on *Advanced*
- Choose Group *Existing* 
- Choose *Existing Database* 
- Choose Deployment *Kubernetes*
- This time, let's choose Node as language
- Click *Cloud Shell*

![Group App2](images/starter-group-app2.png =80%x*)

```
cd dev 
curl "https://starter.wedoteam.io/app/zip?prefix=starter2&deploy=compute&ui=html&language=java&database=atp" --output starter2.zip
unzip starter2.zip
cd starter2
cat README.md
```

Run the build. Itt will take the environment variables from the group_common_env.sh created above.

```
./build.sh
```

Test if it works. Notice, the first app is using Java, the second one NodeJS.

```
http://123.123.123.123/starter/
http://123.123.123.123/starter2/
```

Congratulation, if you reached this point, you created a group of 2 microservices using the same common resources !!

## Acknowledgements 
- **Author**
    - Marc Gueury
    - Ewan Slater 

- **History** - Creation - 30 nov 2022