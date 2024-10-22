# Cloud Computing Homework 1 - Containers and Virtual Machines

## Project Overview
This project aims to compare the performance of **virtual machines (VMs)** and **containers** by running a simple web server using both **Vagrant** for VM and **Docker** for containers, and evaluating them through load testing.

We used **Vagrant** to create and manage both the virtual machine server and a client VM for load testing with `wrk`. Additionally, we used **Docker** to host the server as a container on the host machine, allowing us to compare both methods of deployment.

### Requirements Recap:
- **Server**: One computer running **Vagrant** and **Docker**.
- **Client**: One computer with **Vagrant**.

### Experiments:
1. **Experiment 1**: Host the server using a **virtual machine** created with Vagrant, running **NGINX**.
2. **Experiment 2**: Host the server in a **Docker container** on the host machine.

---

## URLs to Vagrant and Docker Files

- **Vagrant File for Server in VM**: [Vagrantfile (Server)](https://github.com/ronygsp/Vagrant-vs-Docker/blob/main/vagrant-server/Vagrantfile)
- **Docker File for Server in Container**: [Dockerfile (Docker Server)](https://github.com/ronygsp/Vagrant-vs-Docker/blob/main/docker-host-server/Dockerfile)
- **Vagrant File for Client with `wrk` Tool**: [Vagrantfile (Client)](https://github.com/ronygsp/Vagrant-vs-Docker/blob/main/vagrant-client/Vagrantfile)

---

## Commands to Replicate the Load Test

### Experiment 1: Virtual Machine with Vagrant (Server)

1. **To Start the Server with Vagrant**:
   ```sh
   vagrant up --provider=virtualbox
   ```
   This command sets up the virtual machine and installs **NGINX**.

2. **To Start the Client and Conduct the Load Test**:
   ```sh
   vagrant ssh  # Connect to the client VM
   ./wrk -t2 -c100 -d180s http://192.168.33.10:80
   ```
   This command runs a **load test** for **180 seconds** with **100 concurrent connections**.

### Experiment 2: Docker Container on Host

1. **To Build the Docker Image and Run the Container**:
   ```sh
   docker build -t nginx-host-server .
   docker run -d -p 80:80 --name nginx-host-container nginx-host-server
   ```
   This builds the Docker image and runs the container to host the web server.

3. **Alternatively if its already built**
   ```sh
   docker start nginx-host-container
   ```

3. **To Conduct the Load Test from the Client**:
   ```sh
   ./wrk -t2 -c100 -d180s http://[Host_IP]:80
   ```
   
---

## Analysis: Containers vs Virtual Machines

### Results Summary:

- **Experiment 1 (Vagrant/VM)**:
  - **Requests per Second**:
    - Test 1: **3501.23 requests/sec**
    - Test 2: **3100.22 requests/sec**
    - Test 3: **3066.09 requests/sec**
  - **Transfer Rate**:
    - Test 1: **911.36KB/sec**
    - Test 2: **811.24KB/sec**
    - Test 3: **802.31KB/sec**

- **Experiment 2 (Docker)**:
  - **Requests per Second**:
    - Test 1: **4446.79 requests/sec**
    - Test 2: **8748.91 requests/sec**
    - Test 3: **7558.76 requests/sec**
  - **Transfer Rate**:
    - Test 1: **1.74MB/sec**
    - Test 2: **3.42MB/sec**
    - Test 3: **2.96MB/sec**

### CPU and Memory Utilization Comparison:

1. **Virtual Machine Server (Vagrant)**:
   - **CPU Utilization**: The CPU usage reached a significant peak of 91.9%, indicating that the VM struggled under load, resulting in increased CPU resource consumption. This indicates that under heavier loads, the virtual machine was less efficient at balancing resource utilization, likely because of the overhead of running a full operating system.
   - **Memory Utilization**: Memory consumption was higher in the virtual machine (131MB out of 965MB), which is consistent with the overhead of maintaining an entire guest OS, leading to a more resource-heavy environment. The memory usage, though not fully saturated, indicates that running a complete OS can lead to higher baseline memory requirements.

2. **Docker Container (Host)**:
   - **CPU Utilization**: Docker exhibited moderate CPU usage across multiple worker processes, ranging from approximately 3% to 13% per worker. The container was efficient in balancing the workload across different CPU cores, and the CPU utilization was distributed evenly. This behavior shows Docker’s ability to dynamically manage resources, allowing it to efficiently utilize multiple CPU cores.
   - **Memory Utilization**: Docker’s memory consumption was much lower (1.25GB out of 7.46GB), showing efficient resource usage compared to the VM. The lower memory usage is a direct result of Docker using the host operating system’s kernel, avoiding the additional overhead of a full virtual machine OS.
### Key Differences

1. **Performance**:
   - Docker containers were more efficient at managing CPU usage during high-load scenarios, distributing the workload across multiple worker processes. This led to better load handling and consistent CPU usage across cores.
   - Virtual machines peaked at 91.9% CPU usage, showing that they struggled with resource management and had less efficient CPU handling compared to Docker. This behavior indicates that the overhead of running a full OS significantly impacts performance under load.

2. **Efficiency**:
   - The VM environment consumed more memory and peaked at high CPU usage, suggesting that its overhead led to less efficient performance compared to Docker.
   - Docker containers showed lower memory usage, efficient CPU utilization, and managed worker processes better, indicating an optimized use of resources without the additional OS overhead.

3. **Scalability**:
   - Docker containers have an advantage in scalability due to their lower overhead and ability to utilize system resources more dynamically. They can handle workloads efficiently across multiple CPU cores and adjust resource allocation accordingly.
   - Virtual machines, while offering better isolation, face challenges in scalability due to their resource-heavy nature, slower response to increased load, and high memory requirements.

### Conclusion

The experiments demonstrate that Docker containers are generally more **efficient** in terms of **resource utilization**, **response time**, and **handling concurrent requests**. They have lower latency and can process a higher number of requests per second compared to virtual machines. Virtual machines, while providing better isolation, come at the cost of increased overhead.
