# Horizontal and Vertical scaling

### What is scalability?

The scalability of the application can be measured by the number of requests 
it can effectively support at the same time. The point where the application can't handle 
requests  effectively it's a limit of its scalability. The limit is reached when a critical hardware
resource runs out, requiring different and more machines.


#### Note
Scaling horizontal and scaling vertical are same in one point,
that both increasing the infrastructure computational power 


### What is the main difference?

Horizontal scaling means scaling by adding more machines to your pool of resources (scaling out)


Vertical scaling refers to scaling by adding more power(CPU, RAM) to an existing machine (scaling up)


One of the fundamental difference between the two,
is that horizontal scaling requires break our sequential piece of logic
into smaller pieces, that they can be executed in parallel across multiple machines.

In many aspects vertical scaling is easier because the logic doesn't need to change.
Just you're running the same code base in higher-spec machine.

However there was other many factors which we need to know to choose correct approach.


 Databases | Horizontal scaling </br> (scaling out)                                                                             | Vertical scaling </br> (scaling up)                                                                                                                                                     |
-----|--------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
 |     | In database world, horizontal scaling usually base on partitioning data (each node only contains the part of data) </br></br> For example, we can have one node to storing information about electronics, another node for clothing, and another for home appliances. This way, each node only contains data related to a specific product category.| In vertical scaling the data lives inside single node and scaling is done throught multi-core, spreading the load between CPU and RAM rescoures of the machine. </br></br> Example</br> Imagine a small business that has a website running on a single server. Initially, the server has 2 CPU cores, 4 GB of RAM, and 100 GB of storage. As the business grows and attracts more visitors, the server starts to experience performance issues due to increased traffic|

</br>

 Downtime | Horizontal scaling </br> (scaling out)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Vertical scaling </br> (scaling up)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| 
|          | In theory adding more machines to the existing pool means thet we are not limited by the power of one machine, whcih helps scale application with less downtime </br></br> For example,  video streaming service experiences a rapid growth in its user base and needs to accommodate the increased demand for streaming content. To handle the growing number of concurrent users and provide a smooth streaming experience, the service decides to scale horizontally by adding more servers to its existing pool. This allows them to distribute the streaming load across multiple nodes, ensuring that users can watch videos without buffering or delays. ... A social media platform with millions of users needs to manage a large volume of data, including user profiles, posts, comments, and multimedia content. To maintain high performance and support the continuous growth of its user base, the platform scales horizontally by adding more database and application servers to its infrastructure. This helps distribute the load across multiple nodes, reducing the risk of downtime and ensuring a responsive user experience. In vertical scaling the data lives inside single node and scaling is done throught multi-core, spreading the load between CPU and RAM rescoures of the machine. </br></br> | Vertical scaling is limited to the capacity of one machine, scaling beyond that capacity can involve downtime and has an upper hard limit, i.e. the scale of the hardware on which you are currently running. </br></br> A popular news website is hosted on a single server with 8 CPU cores, 64 GB of RAM, and 2 TB of storage. During a major breaking news event, the website experiences a sudden surge in traffic, causing the server to become overwhelmed and unresponsive. Although the website owner can upgrade the server's resources to handle the increased traffic, there's an upper hard limit to how much the server can be scaled vertically. Moreover, upgrading the server may involve downtime, which is undesirable during a high-traffic event. In this case, horizontal scaling would be a more suitable solution to distribute the traffic load across multiple servers and ensure the website remains accessible during peak times. An online video editing platform runs on a single powerful server, which handles all the video processing tasks. As the number of users and concurrent video processing jobs increases, the server's resources become a bottleneck, leading to slow processing times and a poor user experience. Although the platform owner can upgrade the server's hardware to improve performance, there's a limit to how much vertical scaling can be done. Additionally, upgrading the server may require downtime, which could disrupt the service for users. In this situation, horizontal scaling by adding more processing servers to the existing pool would be a more effective approach to handle the increased workload. |

</br>

 Concurrency | Horizontal scaling </br> (scaling out) | Vertical scaling </br> (scaling up)|
-----|-----------|---------| 
|     | Also described as a distributed programming as it involves distributing the jobs across moachines over the network| Actor model: concurrent programming on multi-core machines is often performed via multi-threading and in-process message passing </br> |

</br>

 Message passing | Horizontal scaling </br> (scaling out) | Vertical scaling </br> (scaling up)|
-----|----------------------------------------|---------| 
|     | In distributed computing, the lack of a shared address space makes data sharing more complex. It also makes the process of sharing, passing or updating data more costly since you have to pass copies of the data.  | In a multi-threaded scenario, you can assume the existence of a shared address space, so data sharing and message passing can be done by passing a reference. </br> |


