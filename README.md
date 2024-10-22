Cloud Computing Project: Containers vs Virtual Machines

Overview:

This project is a part of a Cloud Computing class assignment, aimed at comparing the usage and performance of Virtual Machines and Containers for hosting a web server. The project involves setting up a web server using both Vagrant and Docker, performing load tests, and analyzing the resource usage of both approaches.

Project Structure:

Experiment 1: Virtual Machine Server (Vagrant)

Set up a Ubuntu Virtual Machine using Vagrant to host an NGINX web server.

Client VM (also set up using Vagrant) installs wrk to run a load test against the server.

Experiment 2: Docker Container Server (Host)

Set up an NGINX web server in a Docker container running directly on the host machine (not within a Vagrant VM).

The same client VM runs the load test against the Docker container server.

Prerequisites:

Vagrant (with a provider such as VirtualBox)

Docker Desktop (for Docker on the host)

Git (for managing the project repository)

wrk load testing tool

Getting Started:

Experiment 1: Running the Server in a Virtual Machine

Navigate to the Vagrant Server Folder

Start the Server VM

SSH into the Client VM and Run the Load Test against the Server VM:

Experiment 2: Running the Server in Docker on the Host

Navigate to the Docker Folder

Build the Docker Image

Run the Docker Container

Run the Load Test from the Client VM

Stopping the Environment

Stop the Vagrant Machines (Server and Client VMs):

Stop the Docker Container:

Results and Analysis:

The key focus of this project is to compare the CPU and memory utilization and performance (requests per second and latency) of the web server in two environments:

Virtual Machine (Vagrant): Complete virtualized system, running a full OS stack.

Docker Container (Host): Lightweight, sharing the kernel with the host, typically more efficient.

Expected Results

Docker should show lower CPU and memory usage compared to the virtual machine, due to reduced overhead.

Higher requests per second and lower latency are expected when using Docker, as it is more efficient than running an entire VM.

Conclusion

The project aims to provide a clear understanding of the trade-offs between using containers and virtual machines in terms of resource utilization and performance. It demonstrates the benefits of Docker in running lightweight, efficient services and highlights the overhead associated with running full virtual machines.

Repository Structure

vagrant-server/: Vagrantfile to set up the server in a VM.

vagrant-client/: Vagrantfile to set up the client in a VM.

docker-host-server/: Dockerfile and related files to build and run the server in Docker.

License

This project is licensed under the MIT License. See the LICENSE file for details.
