[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: WANG Jing
### Student Id: 21095022
### Email: jwangke@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

   **Measurement Tool: Phoronix Test Suite**  

   **Configuration:**  
   Run the 7-Zip compression benchmark in Phoronix Test Suite, measuring the system's compression and decompression performance. 
   ```
   phoronix-test-suite run pts/compress-7zip
   ```  
   Run a memory performance benchmark in the Phoronix Test Suite, specifically testing the speed of a system's RAM by measuring the read and write speeds.
   ```     
   phoronix-test-suite run pts/ramspeed
   ```
   
   **Measurement Values:**  
  For CPU measurement, I choose Decompression Rating MIPS, which assess CPU's instruction execution speed in millions of instructions per second.  
  For memory performance measurement, I choose the Copy and Integer option to evaluate the performance by processing Interger operation. And the MB/s stands for Megabytes per second and is a unit of data transfer rate or data throughput.  
  I use the default settings for both tests to ensure consistent and dependable benchmarking results for two cases.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro`  | 3158 MIPS       | 11310.45 MB/s      |
    | `t2.medium` | 5902 MIPS       | 19547.19 MB/s      |
    | `c5d.large` | 4861 MIPS       | 13870.55 MB/s      |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

    According to the results, t2.medium's performance is the best, while t2.micro's is the worst with less vCPUs and memory resources. However, the performance of c5d.large is lower than t2.medium although they have the same number of VCPUs and memory. As the following table shows, t2.medium and c5d.large have different system architectures which may account for the performance difference. So the conclusion is the performance of EC2 instances generally increases with the increase of the number of vCPUs and memory resource with other factors fixed.
   
    | Size        |      vCPU	      |    Memory (GiB)    |    Architecture    |
    | ----------- | --------------- | ------------------ | ------------------ |
    | `t2.micro`  | 1               | 1                  | i386, x86_64       |
    | `t2.medium` | 2               | 4                  | i386, x86_64       |
    | `c5d.large` | 2               | 4                  | x86_64             |
   

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 3560           | 0.355    |
    | `m5.large` - `m5.large`   | 4970           | 0.295    |
    | `c5n.large` - `c5n.large` | 9410           | 0.124    |
    | `t3.medium` - `c5n.large` | 2350           | 0.824    |
    | `m5.large` - `c5n.large`  | 3810           | 0.444    |
    | `m5.large` - `t3.medium`  | 1890           | 0.905    |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

    In the same region, the network performance between instances of the same type is better than that between instances of different types.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 31.6           | 59.2     |
    | N. Virginia - N. Virginia | 4890           | 0.268    |
    | Oregon - Oregon           | 4960           | 0.178    |

    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

    The connection speed between EC2 instances across different regions is significantly slower—by an order of magnitude—compared to instances within the same region.
