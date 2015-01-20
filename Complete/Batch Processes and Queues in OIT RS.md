# Batch Processes and Queues in OIT RS

**Job:** An execution of a program (dotnet exe, dll or vbscript file)

**Batch Job:** A job that gets scheduled and/or executed through the batch processing system, which was developed by the Process Control System (PCS) group in house, and was based on the MSMQ technology.

**Batch Process:** Run batch jobs

**Queue (Batch queue):** A container of a group of batch jobs, each batch queue has a queue type. batch servers are associated with queue types

**Queued Batch Job:** A batch job that gets initiated by the QueueIt function of the batch processing system. Queued batch jobs are monitored and controller through the QCP web site. They are queued in predefined queues. 

**Non-queued batch job:** A batch job that gets initiated by the RequestIt function of the batch processing system. Non-queued batch jobs are NOT monitored and controller through the QCP web site. They are not queued in any predefined queues and they usually get executed immediately (synchronously?). The only way to know the status of non-queued job is through UnderCover logs or the SQL records the batch job modifies.

**QCP:** A web site that is maintained by the PCS group and is serving as a dashboard and control panel to view and manage the queued batch jobs. http://rs-qcp.fasttax.com/

## Purpose

* System scalability and stability
	* Move the workload from one server to another, spreading load, easing load balancing
	* Break large jobs into smaller jobs, so that no single job can hold up the CPU for too long
	* The damage of a single failure of a job can be limited to itself so it will not affect other jobs
	* Keep persistence records for the status and progress of the batch jobs (queued jobs only)
* Responsive user experience
	* Asynchronously process jobs and data in the background
	* Control the flow and velocity of the job processing work for different types of users and jobs
* A general convenient way of sending work from one server to another

## Using the Batch Process

1. Create queued batch jobs in code:
	1. Construct a batch processing obect
	2. Pass parameters to the object by setting props (Queue name)
	3. Call the object's QueueIt method
	4. ![Queued Batch Job](http://i.imgur.com/2FLYZ6o.png)
2. Create non-queued batch jobs in code:
	1. Contruct a batch processing object
	2. Pass parameters to object (IP address)
	3. Call the object's RequestIt method
	4. Mainly for running jobs on web server & app server
	5. ![Non-queued batch job](http://i.imgur.com/BKD3GGi.png)
		* Notice that the batch server can send non-queued jobs to the app server as well. 
3. Create queued batch jobs manually, using QCP's Trigger function:
	1. In QCP, create a "Trigger" item
	2. Launch the Trigger item's web page
	3. Set properties on trigger item (Queue name, file name)
	4. Click the 'queue it' button on the item's web page. 
	5. ![](http://i.imgur.com/Wkl3PS2.png)

Most of our compute jobs are using process #1. 



