---
title: "Docker Networking"
seoTitle: "Docker-network"
seoDescription: "basic of docker networking , bridge network and host network"
datePublished: Mon Jun 10 2024 07:43:12 GMT+0000 (Coordinated Universal Time)
cuid: clx8o1wrw000008l5eay68wwd
slug: docker-networking
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1717995042599/6d1605f5-3fbc-43aa-b348-91507306f951.png
tags: docker, devops, docker-network

---

By default docker has below networks :

1. **Bridge network**: This is Docker's default network, which allows containers to communicate with each other. It is isolated from the host network.
    
2. **Host network**: This network uses the host machine's network stack, providing direct access to the host's network interfaces.
    
3. **None**: This option disables networking for the container.
    

when you do ip addr show , you will get the ethernets and the ip address of vm

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717998729251/0e126ddf-dc1c-469c-87f5-b770ab246f67.png align="center")

1\. **enp0s3 (Ethernet Interface)**:

This is the primary IP address 10.0.2.15 assigned to the VM. And subnet 10.0.2.15/24

**2\. docker0 (Docker Bridge Interface):**

This network is created when docker is installed on vm.

The IP address of the Docker bridge network interface is 172.17.0.1/16.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717998702194/942afac7-bd03-4332-bbdb-b49e8338321a.png align="center")

1. **Bridge Network**: Suitable for most containerized applications, especially when network isolation is desired. Commonly used in development, testing, and production environments.
    
    **a) Default bridge network:**
    

Now we are try to learn docker deafult bridge network.

To understand the networking in a better way , let us try to create a container

Now inspect the container to check the ip address assigned to it .

docker inspect container-1

we can see the ip address assigned to docker container is 172.17.0.2

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717997814296/47d15c1f-e1e8-4d32-b937-9379bb621a1d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717997753581/072833a9-253a-45e8-8b6d-59a25f403b64.png align="center")

whenever we dont mention any network during container creation by default it takes bridge network

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718001538474/daa924a4-6f26-4858-b358-be4d04965aec.png align="center")

now create container-2 and check the ip address

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717997719299/54ecb171-ee06-4e65-b26c-554d2b41ab09.png align="center")

login into container-1 using below command

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717998425030/104ca06a-001b-420d-a61d-1d0f9f8b3e01.png align="center")

Try to ping container-2 ( IPAdress 172.17.0.3) , we are able to ping container-2 from container-1 , it means the network connection is established between two containers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717998054237/2d0ff4da-14d6-4fc8-8b1a-05253ca0f8bf.png align="center")

The disadvantage of the default bridge network cannot resolve each other by container names. They can only communicate using IP addresses. But ip addresses changes everytime we create the containers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718003619751/63a28782-6bc9-43f4-9375-a24c95d12272.png align="center")

**b) Custom bridge network:**

Now to isolate the containers in the point of networking , let us create a custom bridge network named demo-bridge .

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718001723507/ae7bd72e-7f4a-4256-85bb-2e35927eb978.png align="center")

when we create demo-bridge network a ethernet gets created with subnet of

172.19.0.1/16

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718002015314/40621238-90d1-4217-afe5-f9dc89fb7166.png align="center")

create container-3 and assign it to demo-bridge network

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718001889456/9d006858-db15-4915-876d-bda0399c197b.png align="center")

and the ip address is 172.19.0.2

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718002314372/47a4cce8-166a-409e-bd10-06ac88fbd806.png align="center")

Now try to ping container-3 which is in demo-bridge network from container-1 which is in default bridge network . It doesnt work as the containers are in different docker networks , thus we can achieve network isolation between containers.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718002438222/4a957d82-ad77-4d8b-8332-3a9c0def23a7.png align="center")

Advantages: Containers on a custom bridge network can resolve each other by container names, facilitating easier communication.

create a container-4 on demo-bridge network

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718003302053/ab27a6d7-21ed-4f50-8e9d-d2d6873a8e77.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718003342232/038e7059-e4cd-4297-a872-81ef6d307e44.png align="center")

login into containe-3 and ping container-4 using container name it is possible to ping. This is because custom bridge network can resolve each other by container names.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718003435410/74c234fc-70c3-4600-ad68-735de314505e.png align="center")

2. **Host Network**: Best suited for applications that require high network performance or direct access to the host’s network, such as monitoring tools or network services that need to bind to the host’s IP addresses.
    
    Create a host-container and assign it to host network . We can see there is no IPAddress is created for container.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718002672765/e696d1b4-76a3-4c97-a0d2-340e8dcbc379.png align="center")
    
    The container is attached directly with host network instead of docker network. You can access the "host-container" nginx page by entering localhost.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718002746319/84e56b59-9d56-4939-b428-70d8e9f0c4e6.png align="center")
    
    Find the diagram for the networking of above container
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1718007479885/238e7d0d-7153-4af4-b4e0-1852ee56aa09.png align="center")