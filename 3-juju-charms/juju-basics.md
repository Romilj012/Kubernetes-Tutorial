Juju
- Juju is like a supercharged Operator framework. It helps we deploy and manage complex applications on Kubernetes (and other platforms) easily.

- Why do we need Juju?
    Operators are great, but writing them from scratch is hard.
    Juju provides a way to create and share Operators (called Charms) for any application.

- juju has command line interface
- we can easily deploy things both from cloud but also kubernetes deployment itself we can deploy stuff we know without having to think about what the command is
Charm

- A Charm is a pre-built Operator created using Juju. It contains all the instructions needed to deploy and manage an application.

Why do we need Charms?
1. They save time: we don’t need to write Operators from scratch.
2. They are reusable: we can share Charms with others.
3. They are modular: we can combine multiple Charms to deploy complex systems.


Example:
Imagine we want to deploy a WordPress website with a MySQL database. Instead of writing our own Operators, we can use:

- A WordPress Charm to deploy WordPress.

- A MySQL Charm to deploy the database.

- Juju will handle the deployment and ensure everything works together


- charm is a encapsulation of a piece of software in our case it's going to be a piece of software that we write it's called say Q and we're going to deploy that into a kubernetes cluster just to demonstrate a very simple way of being to deploy this software

- we can combine cloud based charm deployement with kubernetes charm based deployment

- most of the charm files have
1. name
2. summary 
3. who maintains it 
4. description
5. requires is that we can connect mysql to the charm and it also exposes mysql to the rest of the world. requires end it will allow us to do something with that mysql like create database, setup an account all kinds database stuff

we also have a layer.yaml to include all the kubernetes ready 
- caas - container as a service
- docker-resource
- interface - juju-relation-mysql (for maling database)

how to deploy kubernetes charm to my cluster
- Use a mutipass terminal for ubuntu vm 
- charm build - download all the layers
- charm push  take the pathway it's built the output to so the destination charm directory and then we tell it what namespace we want it to be in so in our case it's spicule charms /k8s that will go and push the latest changes
- then attach my docker image to it and now I can just decide whether to release it onto the edge branch the betta branch


now going to bootstrap a new control just for this demo to show we how it works I've already added the kubernetes cloud to my OpenStack infrastructure 
![alt text](image.png)

- juju status
- do juju add-model 
- then deploy the model using juju deploy model name

![alt text](image-1.png)

- we can use watch --color juju status --color
- juju debug-log
We can see exactly what the containers trying to print out while it's trying to do and so as it's going through its initialization process and standing stuff up we can make sure that our container is configuring itself properly is we know setting up all the we know relations and that type of stuff if there's some issues it will log it here and we can just make sure that everything is working as we see fit
![alt text](image-2.png)


![alt text](image-3.png)

- juju add unit
- juju delete application
- juju add -relation  mariadb-k8s saiku-k8s


![alt text](image-4.png)