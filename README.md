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
   ./wrk -t2 -c100 -d180s http://172.17.112.1:80
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
    - Test 1: **0.89MB/sec**
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
   - **CPU Utilization**: The CPU usage stayed consistent at relatively **low percentages** throughout the load testing. It handled a moderate amount of requests but lacked the spikes and adaptability seen in Docker. It experienced higher latency as the load increased, indicating its slower response time.
   - **Memory Utilization**: The memory usage was steady, generally using more resources than Docker due to the added overhead of running a full operating system.

2. **Docker Container (Host)**:
   - **CPU Utilization**: Docker containers exhibited **higher CPU usage**, with dynamic utilization that peaked during heavier loads. The **CPU usage reached 50-55%**, which indicates more efficient resource handling due to the reduced overhead.
   - **Memory Utilization**: Docker's memory consumption was **lower** compared to the VM, making it a better option for environments where resources are limited. The containerized approach avoids the resource-heavy nature of running a complete guest OS.

### Key Differences

1. **Performance**:
   - **Docker containers** were able to handle significantly more requests per second compared to the VM. This is because containers share the host’s OS, which eliminates the need for managing additional OS layers, reducing latency and resource consumption.
   - In the **VM**, the latency increased with higher concurrent connections, leading to a less responsive environment under heavy loads.

2. **Efficiency**:
   - The VM environment showed consistent resource usage but with increased **latency** and **lower throughput**. The **higher latency** in handling requests suggests that the virtual machine's overhead affects its efficiency.
   - **Docker** was more resource-efficient, had faster startup times, and could handle a greater number of requests per second. It used **less memory** compared to the VM, showing that containers are well-suited for rapid scaling.

3. **Scalability**:
   - **Docker containers** have advantages in environments requiring rapid scaling and quick startup, whereas virtual machines take longer to provision and are heavier in terms of resource use.

### Conclusion

The experiments demonstrate that Docker containers are generally more **efficient** in terms of **resource utilization**, **response time**, and **handling concurrent requests**. They have lower latency and can process a higher number of requests per second compared to virtual machines. Virtual machines, while providing better isolation, come at the cost of increased overhead.
