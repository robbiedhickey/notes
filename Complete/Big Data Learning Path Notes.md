# Part 1: Overview of Big Data

**Topic 1**: Overview of Big Data | In this section you have 2 videos and an article to go through

**OBJECTIVE**: To demystify the buzz around Big data. Be able to identify some basic terminologies & the business impact of Big Data and hear from our senior technology leaders on why they think it is important for Thomson Reuters

## Big Data - Thomson Reuters Overview

Explores the trend of big data. It allows us to process and interpret unstructured data. 

Thomson Reuters is leveraging big data for predictive analytics and visualizations to add value to existing products. It is gaining steam because the hardware necessary to process big data is becoming cheaper every year, and the toolsets are maturing. 

We used to expect customers to solve the data integration problems. 

Thomson Reuters is now evolving from a data provider to a provider of intelligent information. The exciting aspect is thinking about what new kinds of products and services can be produce by leveraging and analyzing our existing data. 

Legal&IP leverages machine learning to improve their search. They effectively molded their algorithm to behave like a lawyer. 

Example: some clients want to track the public sentiment of certain keywords/stocks/etc. So we provide a visualization that is on a timescale, so you can see where the sentiment is trending. 

Challenge is to deliver the information in a way that allows the clients to mashup their disparate datasets into the content. 

Another question: Can you forecast global crude oil supply and demand? Many customers want this. How many ships are heading to the US? Is it different from last year? Is it different from a week ago? We have over 10 million bill of ladings that come in every year, so sift through those and find the crude oil records. Next step is visualization. 

### Three V's of big Data

* Volume - volume of data in world doubles every two years
* Velocity - often reaches us in real-time
* Variety -different structures and schemas, or often no structure at all

## What big data says about you? (THINKR video)

Some people describe big data as the planet developing a nervous system. Up until 2003, the entire history of data collected for recorded history was 5 exabytes. Now we are generating 5 exabytes of data every 2 days. Just about everything in our lives generates data. 

We are in the infancy of big data, essentially where the internet was in 1993.

Example: In years prior pharma companies might develop a drug that is 99.9% effective, but could not release it because of the .1% that it could adversely affect. DNA sequencing is getting cheaper every year, expected to be closes to a $100 5 years from now. When this occurs, pharmacists will be able to analyze your genome sequence to see if you would be adversely affected by a given drug. In effect, pharma companies have cures sitting on the shelves that cannot be used until this is a reality. 

Also consider data considered worthless to one group might be gold to another. Air traffic controllers have been throwing data on bat migrations because it interferes with their readings. Scientists would love to get their hands on this data. 

## Big Data Blast Off (BBC Article)

### The numbers

1,000 bytes = one kilobyte (kB)

1,000 kB = one megabyte (MB)

1,000 MB = one gigabyte (GB)

1,000 GB = one terabyte (TB)

1,000 TB = one petabyte (PB)

1,000 PB = one exabyte (EB)

1,000 EB = one zettabyte (ZB)

1,000 ZB = one yottabyte (YB)

### Can we handle it?

The job of data scientist didn't exist 5-10 years ago. There is a huge shortage and education and businesses are trying to catch up. 

### Who owns the data?

Still a legal minefield. Privacy and IP laws have not kept up with the pace of technological change.

# Part 2: Big Data Landscape

**Topic 2:** Big Data Landscape in Thomson Reuters | In this section you have 7 videos to go through

**OBJECTIVE:**  To get introduced to the three broad Big Data groups we have in Thomson Reuters and some key projects that they are currently working on.

## Overview of Big Data Landscape by Mike Edwards

### Three broad Big Data groups in Thomson Reuters

* DCO - Data Center Organization.
	* Responsible for the infrastructure needed to run and analyze big data. 
	* Have a number of Hadoop clusters in the data centers
	* also have R&D groups within to explore new methods
* Platform group
	* building reusable services and R&D
* Business Units
	* Customer facing groups, top of the stack. 

## The Research Group by Tom Zielund

Works in Emerging Technologies. 

### What are the big data projects in Thomson Reuters?

* Project BOLD
	* Big Online Linked Data - gather data from all over TR, put it all into a data lake, and run analytics on it
	* Enable cross-usages
* IP&S group - Web of knowledge
	* leverages hadoop
* Legal - Westlaw next
	* uses hadoop to process user experience data
* Starmine quants
	* use big data to determine financial signals
* IP&S InCites
	* graph data techniques
	* visualization
* Usage analytics
	* personalize westlaw next experience
	* personalize news recommendations
	* Group users together in Eikon based on the documents they've read

## The DCO Group and their Big Data story

* Role of the DCO group
	* Evaluate new technologies
	* Select standards
	* Build and operate infrastructure
	* manage licensing
	* Provide consulting for business units and dev teams

### The DCO Middleware Group

* Provides infrastructure services to manage BU big data clusters
	* Hadoop and HBase
* Services include:
	* Provision the hadoop cluster OR
	* Just provide slices on the multi-tenant cluster
	* Change and incident management
	* Design and engineering guidance
	* Complete life cycle management
		* Tech refresh
		* Software upgrade

#### How and when to reach out to DCO middleware team?

Reach out to your BU architect. They will decide whether big data is an initiative for the team, and will collaborate with the DCO infrastructure architects. Then a high level infrastructure solution diagram is provisioned. It is essentially a technical representation of your business problem. It usually takes 2 weeks to provision the cluster once the hardware has arrived. The number of nodes can vary from 5-50+. SLA does not vary based on these numbers.

## The BU Implementers/Developers and their Big Data story!

### Business Case from the Content Marketplace Group

Works with Public Records data. They generally contain information about people and organizations and include:

* Credit headers
* Phone records
* Licenses
* Assets

They leverage over 4.5 billion documents that are currently used by CLEAR and WestlawNext

#### Our Big Data Challenge

Process all of the public record data and decide what information was associated to what company or person. This consisted of comparing all documents against a database of 500 million people and 35 million companies. People data generated 30 billion relationships, while company generated 6 billion relationships.

Now we can ask questions: Within a particular timeframe, find all the neighbors within 1 square mile of this person? Given an organization, does anyone have an association to someone with a fraudulent background?

#### How do we build it?

Published the documents to Novus. Wrote Java and Hadoop processes to parse/cleanse/associate and load the data. Loaded the data into Oracle Exadata warehouses. Published them to Oracle data marts, which are then consumed by customer applications (CLEAR, etc). 

#### Result

Now we have a reusable platform to conduct people and organization investigation and research. 


### Business Case from the Legal Team

Westlaw Next was built on the Cobalt platform. They collect over 50 GB/day of raw user experience data. Now have over 8 TB of raw data.  All data is stored as JSON objects, and there are over 500 event types. Reporting on JSON with standard Oracle blob methods proved to not be useful.

Today they stored the data in HDFS, which opens up the data to the Hadoop environment. It can be accessed via Map/Reduce, PIG, HIVE SQL and others. 

Several Map/Reduce jobs are scheduled:

* WestlawNext Practitioners page -> Most popular documents 
	* determined by usage over a 3 month window
	* based on a weighted usage algorithm
	* score is calculated nightly and published to the online system. 

They have also opened the data to Adhoc analytics, and expose the data to a product called Datameer which is an excel like analysis tool.  It can allow them to determine if any users are abusing their subscriptions, and can be done in minutes. 

Prior to hadoop only very limited reporting could be done, and this was extremely time consuming. Today, Datameer can analyze an entire year of data and report back in less than an hour. 

### Business Case from the DCO Group

Working on the Kraken Project. It is an F&R product that takes transaction and metadata and runs analytics on it. 

#### How do we build skills? 

The Kraken dev team was new to Hadoop. DCO offers a 'small stack' that is hosted in the private cloud. It is a 5 node sandbox that allows new developers to write map/reduce jobs without being exposed to the shared environment. 

# Part 3: Data Scientists

**Topic 3**: Role of a Big Data , Data Scientist | In this section you have 4 videos and two articles to go through

**OBJECTIVE**: To identify the different roles of a Data Scientist and what they do

## How to Build a successful Data Science Team Article

Don't try to find one superhuman who does it all. You need three experts: business analyst, machine learning expert, and data engineer.

Organizations may be trying to recruit a unicorn. Hire a data science team vs a data scientist which are exceedingly rare. 

### Business Analyst

This person works with front-end tools, meaning those closest to the organization's core business or function, such as Microsoft Excel, Tableau Software's visualization tools, or QlikTech's QlikView BI apps. A business analyst might also have sufficient programming skills to code up dashboards, and have some familiarity with SQL and NoSQL.

They analyze business-level data and try to produce actionable insights.

### Machine Learning Expert

A statistics-minded person who builds data models and makes sure the information they provide is accurate, easy to understand, and unbiased. They develop algorithms and crunch numbers. 

### Data Engineer

They are the ones who play with Hadoop, MapReduce, HBase, Cassandra. These are people interested in capturing, storing, and processing this data… so that the algorithm people can build models and derive insights from it.

## Data Science and Predictions Article

### Key Insights

* Data science is the study of the generalizable extraction of knowledge from data
* A common epistemic requirement in assessing whether new knowledge is actionable for decision making is its predictive power, not just its ability to explain the past
* A data scientist requires an integrated skill set spanning mathematics, machine learning, artificial intelligence, statistics, databases, and optimization, along with a deep understanding of the craft of problem formulation to engineer effective solutions.

### "What data satisfies the pattern vs What patterns satisfy the data?"

Traditional database methods are not suited for knowledge discovery, they are optimized for fast access and summarization of data, given a query - not discovery of patterns in massive data sets where the user lacks a well-formulated query. 

### Implications

Predictions must be falsifiable in order to be valuable. Falsifiability or refutability of a statement, hypothesis, or theory is an inherent possibility to prove it to be false. A statement is called falsifiable if it is possible to conceive an observation or an argument which proves the statement in question to be false.

Given a large trove of data, the computer taunts us by saying, 'If only you knew what question to ask me, I would give you some very interesting answers based on the data.' This is powerful because we often do not know which question to ask. 

For example, consider a health-care database of individuals who have been using the health-care system for many years, where among them a group has been diagnosed with Type 2 diabetes, and some subset of this group has developed complications. It could be very useful to know whether there are any patterns to the complications and whether the probability of complications can be predicted and therefore acted upon. However, it is difficult to know what specific query, if any, might reveal such patterns. 

To be concrete, we could look at date of patient contact with doctor, date of medication dispensed, date of diagnosis, and subsequent cost of treatment. You can think of this as a "clean" period, "diagnosis" and "outcome" period. A learning algorithm could be used on the left to determine the pattern of the data on the right. Putting that data into a canonical form to be analyzed is called 'feature construction'. 

Also consider "robustness" of patterns. A robust pattern is predictive. 

One conclusion which may be predictive is: What is the incidence of complications in Type 2 diabetes for people over age 36 who are on six or more medications?

Predictiveness models should favor Occam's razor, or succinctness, since simpler models are more likely to hold up on future observations. 

Need to be careful about assigning causation as the real cause may not be captured in our observed set of variables. 

### Skills

Machine learning skills are becoming necessary for data scientists in order for companies to build automated decision systems that hinge on predictive accuracy.

Knowledge of "text-processing" or "text-mining" in light of unstructured data. 

Knowledge about markup languages and their derivatives as content becomes tagged.

Machine learning knowledge can be broken down into sub-categories:

* Statistics
	* more specifically, Bayesian statistics - which requires a working knowledge of probability, distributions, hypothesis testing and multivariate analysis.
	* it can be acquired in a two-or-three course sequence
	* multivariate analysis often overlaps with econometrics
* Computer Science
	* Data structures, algorithms, and systems, including distributed computing, databases and parallel computing.
	* scripting languages
	* system skills are fundamental building blocks for working with reasonable-sized datasets.
* Correlation and Causation
	* observational data usually limits us to correlations, sometimes we get lucky
* The ability to formulate problems in a way that results in effective solutions
	* Ex. Many recursive problems can be expressed as the standard Towers of Hanoi problem.
	* keep isomorphism in mind (identical underlying structure)
	* see commonalities across very different problems

Organizational challenge in that management must consider data driven approaches instead of intuition and other past practices. 

### Knowledge Discovery

> "All models are wrong, but some are useful" - George Box

An interesting point on correlation: Google’s language translator does not 'understand' language, nor do its algorithms know the contents of webpages. IBM’s Watson does not 'understand' the questions it is asked or use deep causal knowledge to generate questions to the answers it is given.

Patterns emerge before reasons for them become apparent. Big data makes it feasible for a machine to ask and validate interesting questions humans might not consider.

Physical Sciences are characterized by predictive models that are causal and 100% accurate. It can be used to predict what will happen if any input changes. 95% correct is not sufficient. A physics theory must be complete with no exceptions. 

Social science are generally characterized by more incomplete models intended to be partial approximations of reality. A model correct 95% of the time in this world is quite good. The interesting side effect is that these predictive models, if sufficiently accurate, could point us in the direction of the 'theory' that would eventually result in a causal model. 

The contrast being, we don't require a causal model up front, but we can potentially derive it from simple but accurate predictive models. Basically induction vs deduction. 

#### What makes predictive models accurate, or, Where do errors come from?

Errors come from three sources:

* Misspecification of a model
	* a linear model that tries to fit a nonlinear problem
* The samples used for estimating parameters
	* the smaller the samples, the greater the bias
* Randomness
	* can even occur when the model is specified perfectly

Big data allows data scientists to significantly reduce the first two twpes of errors. Large amoutns of data allow us to consider models that make fewer assumptions about functional form, as opposed to linear or logistic regression simply because there is more data to test such models. Also, sample estimates become reasonable proxies for the population. 

Still, the data is generally passive. Using the health care example, it does not tell us what could have happened if some other treatment had been administered to a specific patient or an identical patient. That is, it the data does not represent a clean controlled and randomized experiment, so Randomness is still a factor for error. 

## Intro to Data Science (Udacity)

### What is a Data Scientist?

Person who is better at statistics than any software engineer and better at software engineering than any statistician. 

### What does a Data Scientist do in theory?

This represents the instructor's point of view. 

1. Collect data from the real world
2. Process it and create a dataset that can be analyzed
3. Analyze trends in existing data, or try to make predictions about the future using the data at hand. 
4. Based on these models/predictions, may build data-driven products, or communicate their findings via reports, visualizations or blogs.

### What does a Data Scientist do in practice?

Asking real data scientists

In reality it is hard to say because it will always be tailored to the actual application area. In general they take data and find meaning in it. Use data to explain and/or predict behavior. Behavior could represent a human being or an abstraction like a market. 

### Solving real life problems with Data Science

Data science can solve problems you'd expect:

* Netflix
	* collaborative filtering algorithms to recommend movies to users
* Social media
	* recommending new connections, etc
* Dating Websites
	* enhance user experience and to publish interesting anthropological findings. 

And a ton of problems you might not expect:

* Bioinformatics
	* annotating genomes, analyzing data sequences
* Urban planning
	* attempted to solve Chicago's bus crowding issues
* Astrophysics
	* compiling 100 TB of astronomical data collected by SLOAN
* Public Health
	* Analyzing electronic medical records allows Camden, New Jersey to save money by targeting their efforts toward specific buildings accounting for majority of admissions
* Sports
	* NBA teams installing cameras to collect movement of players and playing styles. 

# Part 4: Intro to Hadoop and MapReduce (Udacity)

**Topic 4:** A UDACITY course  - In this section you have 19 short  videos clubbed into 3 - to go through

**OBJECTIVE:**  To have a basic and a high level understanding of two key technologies in Big Data namely Hadoop and MapReduce

## Big Data Introduction

Data Sources include Phone data, online stores, medicine, research, etc. Many new sources every year. How do we store and process this large amount of data?

The first thing you need to decide is: Do you really have "Big Data"?

One definition of big data could be: data that is too big to be processed on a single machine. 

### Challenges with big data

* Data from different sources in various formats
* Data is created very fast

## Big Data and its 3 V's

### Volume

* price to store data has dropped incredibly
* ~$.10/GB - but reliable storage often costs more, which puts a cap on the amount of data they can practically store
* however, the data you may be throwing away is incredible useful. All data is worth storing.

Hadoop enables us to store all of this data on cheap commodity hardware, without requiring many thousands of dollars to be invested in SAN infrastructure. 

### Variety

To store data in RDBMS's the data must fit in schemas. This presents a problem of assembling disparate structured data. That is not even considering unstructured data. 

We want to store the data in its original format so that we do not lose any information. Hadoop doesn't care what format the data comes in. 

### Velocity

We need to be able to accept and store data, even when it is coming in at rates as high as TBs/day. If we can't store it as it arrives, we end up discarding some of it.  

For example, if an online retailer knows that you spent 5 minutes looking at a product, they can later send you an email when it is on sale. 

## Intro to the Hadoop Ecosystem

Doug Cutting is one of the founders of Hadoop and chief architect at Cloudera.

### Origin of Hadoop

In 2003, Doug was working on an open source web search engine called Nutch. Knew it needed to be scalable due to the data size. Got things up and running on 4-5 machines and not very well. Around that time Google published their seminal paper on MapReduce and distributed file systems. So his partner and him set about trying to re-implement these in open source. It took them a couple of years and eventually got Nutch up and running on 20-40 machines. Realized that in order to get it to scale to thousands of machines they needed more resources, so he went to work for Yahoo. 

Pulled the parts out of Nutch related to distributed computing and put them into a new project 'Hadoop'. Over the next couple of years Hadoop got to the point where it scaled to petabytes. It spread to a lot of companies in the internet sectors. 

Many projects began to grow up around it, Hadoop is basically the kernel for Big Data. There are tools that allow you to more easily do MapReduce programming (PIG, etc). Also have the beginnings of the higher level tools like Impala and search. Hadoop is becoming more and more a general purpose platform.

The Hadoop logo (yellow elephant) comes from Doug's son's toy elephant. His son was calling it Hadoop. Now his son is 13 and expects royalties for the name!

### Core Hadoop

Two distinct responsibilities:

* STORE data in HDFS
	* split the data up among machines in a cluster
	* can add more machines to the cluster as your data grows
	* machines don't need to be high end. 
* PROCESS data with MapReduce

### Hadoop Ecosystem

Tooling that has grown up around Core Hadoop. Some are designed to make data easier to get into a hadoop cluster, while others are meant to make hadoop easier to use. 

#### Hive and Pig

Writing MapReduce code is not very simple. You need to know a programming language such as Java/JavaScript/Python/Ruby/Perl. There are many data people who know SQL but do not know a programming language. You also have a lot of BI tools that want to hook into Hadoop, which are used to SQL syntax. 

Hive and Pig were created as a response to these issues. Hive statements look much like SQL, and the Hive interpreter will compile your statements into their MapReduce equivalents. Pig is a simple scripting language. 

They are both great, but are still running MapReduce jobs. 

#### Impala

Impala is a way to query Hadoop with SQL but directly accesses the data in HDFS, so is not MapReduce. Impala is optimized for low-latency queries which run over and over again, while Hive queries are optimized for long running batch jobs. 

#### Sqoop and Flume

Sqoop takes data from a traditional RDBMS and puts it into HDFS as delimited files.

Flume ingests data from external systems and publishes it to HDFS.  

#### HBase

A real-time database built on top of HDFS. 

#### Others

* Hue is a graphical front-end to the cluster
* Doozie is a workflow management tool
* Mahout is a machine learning library

There are so many tools that making them talk to one another can be tricky. To make working with these technologies easier, Cloudera has a distribution called CDH which packages them together so installation is easy. The components are also tested extensively. 

# Part 5: Intro to NoSQL

## What is NoSQL? (Youtube)

A NoSQL database provides a mechanism for storage and retrieval of data that is modeled in means other than the tabular relations used in relational databases.

Main characteristic is its non-adherence to RDBMS concepts. Means "Not-only SQL". Grew with internet giants. 

When an RDBMS has gigantic volumes of data, latency and response times become a problem. To overcome this NoSQL aims to distribute load to multiple hosts (Scaling out). NoSQL databases are designed to scale out and often do so better than RDBMS. 

They do not use SQL to query data and do not follow strict schema. 

ACID (Atomicity, Consistency, Isolation and Durability) are not always guaranteed. 

### Why it makes sense to learn SQL after NoSQL?

NoSQL are highly specialized systems, and have special use cases. The vast majority of use cases still prefer RDBMS. 

## What is NoSQL? Thomson Reuters Context

### What/Why/How of NoSQL

* Schema-free
* JSON/ Key-value pairs
* REST
	* common but not obligatory.
	* often communicate via REST services
* MapReduce
	* computing paradigm which breaks computation down into distributed slices
* Scale and Replication

#### Why?

Data is often messy. Not every data point has everything in it. Especially when you start consolidating data sets. 

#### How to learn more about it?

The Hub has a TeckKnow series on Big Data. Can also come to Corporate R&D to discuss your data problems

### Thomson Reuters Context

Use them on a daily basis. CouchDB, MongoDB and Cassandra are all used in projects. We are mining twitter to digest social media information to turn it into valuable information for F&R and IP&S. Twitter has 400 million tweets per day so this is a massive amount of information to collect. Thomson Reuters stores a subset of that data in a MongoDB database. 

### Drawbacks/Challenges

There's an awful lot of them to begin with, so making the right choice can be difficult. 

Will all of these databases eventually survive? Are open source developer communities sustainable?

### Additional tidbits

Keep in mind that NoSQL is intended to be beyond SQL or 'Not Only SQL', not to replace it. 

## NoSQL Whitepaper from CouchBase (OPTIONAL: not included in assessment)

> Why NoSQL? Three trends disrupting the status quo http://www.couchbase.com/sites/default/files/uploads/all/whitepapers/NoSQL-Whitepaper.pdf

### Summary

* The number of concurrent users skyrocketed as applications increasingly became accessible via the web (and later on mobile devices).
* The amount of data collected and processed soared as it became easier and increasingly valuable to capture all kinds of data.
* The amount of unstructured or semi-structured data exploded and its use became integral to the value and richness of applications.

## NoSQL Whitepaper from MongoDB (OPTIONAL: not included in assessment)

> NoSQL databases explained http://www.mongodb.com/nosql-explained 

### Summary

When compared to relational databases, NoSQL databases are more scalable and provide superior performance, and their data model addresses several issues that the relational model is not designed to address:
* Large volumes of structured, semi-structured, and unstructured data
* Agile sprints, quick iteration, and frequent code pushes
* Object-oriented programming that is easy to use and flexible
* Efficient, scale-out architecture instead of expensive, monolithic architecture

# Part 6: Big Data Analytics

**Topic 6:** Big Data Analytics | In this section you have  a videos , a Hub Page and a WebEx Recording to go through

**OBJECTIVE:** To be able to list some of the tools and techniques that are used in Big Data Analytics while exploring the future of  its positive impact on business

## Intro to Big Data Analytics by Isabelle Moulinier

She specializes in search and analytics.

Analytics is about:

* Understanding
* Organizing
* Gaining Insights

Big data analytics is about reducing the data to achieve the original goals of analytics. 

### Three aspects of Big Data Analytics

These are sequential, you can't achieve one without having the subsequent. 

1. Descriptive Analytics (counting)
	* allows you to summarize your data, to condense information
	* counting stuff
	* ex. usage data (Westlaw next)
2. Predictive Analytics (modeling)
	* Once you understand what your data is about via Descriptive analytics you can achieve predictive analytics.
	* trying to build a model of your current data so you can apply that model to new data you have not seen. 
	* prediction is not telling you the future, it is telling you what you think the future may look like with some probability attached to it. 
	* Some techniques
		* Statistical Models
		* Data Mining
		* Machine learning
	* Examples
		* Sentiment analysis (positive vs negative)
		* Recommendation (TR has several recommendation systems in legal domain)
		* Amazon (item recommendations)
3. Prescriptive (recommending)
	* The holy grail of big data analytics. 
	* goes one step further than predictive, in that it tries to give the user a **recommendation to act**
	* Examples: decision support systems in medical and health domain

### Another aspect

Transforming unstructured data into structured data. Unstructured data is considered 'text' in this case. 

Example: extract company mentions on twitter or relationships between companies in text. 

## Tech Know Track WebEx: Machine Learning by Frank Schilder

TR R&D is doing a lot of machine learning. Interested in big data to scale up machine learning

### What is big data?

It has been described as the most confusing tech buzzword of the decade. 

Argues that 'big data' should be called 'big analysis'.

#### Three steps in Big Data

1. STORAGE
	* Cassandra - wide-column stores
	* Neo4j - graph database
	* CouchDB - DocumentDB with web access via javascript
	* MongoDB - Document stores
	* OrientDB - Multi-modal DB (combines graph and document)
	* Voldemort - Key-value stores
2. PROCESS
	* Hadoop/Hive
		* Batch processing MapReduce
	* Google Pregel
		* large scale graph computing
		* open source implementations: GoldenOrb, Bagel (Spark)
		* supports big data processing of graphs
	* Google BigQuery
		* open source implementation: Dremel
		* for processing queries quickly
3. ANALYZE 
	* Ask questions
	* Analyze data using machine learning techniques
	* Examples:
		* Time series predictions
		* fraud detection
		* recommendation engine
		* sentiment analysis
		* risk modeling
		* social graph analysis
		* network monitoring
	* Tools
		* Shogun (Support vector machines)
			* large scale machine learning tool (http://www.shogun-toolbox.org/)
		* Mahout
			* Clustering and classification with Hadoop (http://mahout.apache.org/)
		* Spark (no MapReduce)
			* Fast data analysis tool using Scala, Hive and Pregel implementations (http://www.spark-project.org/)
		* R-based systems
			* RHadoop
			* RHIPE

### Proof of concept study

* Challenge
	* Analyze the consumption of our financial data feeds to our customers
	* Process RIC (ticket) data of 4000 devices at 2000 sites
	* Process 2 terabytes of data every month
* Solutions
	* Use hadoop for processing the data and Pig for querying the data
	* Utilize the Simpson cluster for running hadoop
* Outcome
	* Better understanding which customers use which data
* Next
	* think about how we can use this data to gain value for customers

### Conclusions

* Create lots of new big data sets
	* Big data helps us answer questions about
		* Our own data
		* Our customer's data
* What to do next?
	* Learn about new NoSQL data storage techniques (ex Cassandra)
	* Try out big data processing techniques (ex Hadoop)
	* Think about questions your data can answer

## The Future of Predictive Analytics

> 10 predictions for analytic decision support by 2015 http://www.informationweek.com/software/information-management/10-predictions-for-analytic-decision-support-by-2015/d/d-id/1092021?

### Hyper Competition will proliferate

**To unleash the true potential of analytics, companies will have to move beyond the traditional frontiers of pricing, marketing and risk, using analytics to compete on innovation and relatively obscure areas of the business.** Companies in all industries, be it banking, insurance, retail or airlines, will have to innovate in supply chain management and new-value creation to stay competitive on price and quality, and to generate enough capital to outlast competitors.

### Companies Will Compete on Consumption of Analytics, Not Creation of Analytics

Analytics will become more commoditized, with organizations competing on consumption of analytics rather than creating analytics from scratch. **Context and relevance will be the key enablers for effective consumption of analytics.** Organizations will have to focus on various competencies to get consumption of analytics right.
 
**The increasing use of intuitive technologies such as visual analytics, interactive dashboards and decision-support simulation tools is indicative of the trend toward consumption of analytical insights.** Flex-, Silverlight- and Ajax-based Rich Internet Applications are enriching these user experiences. Companies such as Tableau Software, Tibco Spotfire and Qlik Technologies have pioneered visual analytics as a means to give business users quick insight into data.

### New Data Sources Will Emerge

**New data providers will emerge focusing on intelligently interpreted data** -- especially for social media analysis, location data, etc. Multiple Scores (such as FICO for credit, HICO for health, and so on) will be created to better understand human behavior. Web 2.0 business models exploiting network effects will lead to an exponential increase in user-generated data (with FourSquare and Twitter being current examples). Smartphones will monitor our vital signs and chronic conditions, yielding a wealth of data for disease prediction and medical research.

User-generated content will empower levels of prediction that were never before possible. "If we look at enough of your messaging and your location and use artificial intelligence, we can predict where you are going to go," recently commented Eric Schmidt, CEO of Google.

**An increasing proportion of analytic analyses will rely on unstructured data such as text and video.** Social media data (Radian 6, Viral Heat and so on) and RFID data will be merged with location information for supply chain analytics. Similarly, text mining will be used for social media analytics, customer voice data for customer-service and call-center analytics, and video data of customers’ emotional responses to products/store layouts, traffic monitoring and so on for retail analytics.

### The Role of 'Chief Analytics Officer' Will Emerge

The CAO or their effective equivalent will look override organizational and departmental boundaries to **build data-driven competencies wherever information-driven decisions are made**. The expertise of professionals from varied disciplines such as technology, applied math, anthropology, behavioral economics and so on would be applied to decision making.

### Open Source Analytics and Analytics-as-a-Service Will Gain Adoption

Open Source analytics platforms will emerge to increase adoption and better mine the wisdom of crowds. Open source R has already emerged as a leading platform for statistical innovation and collaboration both in academic and industry circles. Adoption is evidenced by commercial vendors of R, such as Revolution Analytics, focusing on scaling the R computing language. There is also a trend of collaboration between proprietary and open source platforms. For example, Tibco Spotfire and SAS now offer the option to call R functions or scripts within their environments. This signifies a shift towards loosely coupled, open computing platforms.
 
**Analytics-as-a-Service will be commonplace and will take different forms, including analytics services providers, analytics focused SaaS companies, existing IT services, system integration and data providers moving into value added analytics services.** Outsourcing and global delivery will yield significant supply side benefits to the analytics industry.

Big players such as IBM and Accenture have already turned their attention to analytics. IBM, for example, has acquired SPSS and it opening analytics centers in India, China and elsewhere. There is an increasing trend of companies in the Fortune 500 issuing analytics RFPs. Analytic Software-as-a-Service models are being deployed in many areas, including Web analytics (Google Analytics and Adobe Omnitur), marketing analytics (M-factor), and hosted/on-demand business intelligence platforms (Panorama, SAP BusinessObjects On Demand, etc.).

### Convergence and Collaboration Will Be the Rule

As new business models emerge and the boundaries of the value chain are blurred, a new era of convergence in the use of analytical techniques and frameworks will come into play. Cross-industry and cross-domain learning will lead to significant breakthroughs in the development and deployment of analytics solutions.

New business models are accelerating the need for convergence and collaboration. As an example, we've seen Microsoft enter retail, cellular phone networks enter the Netbook category, Dell moving from custom configurations to prebuilt offerings. Scottrade and Yahoo are collaborating on data and analytics to optimize lead generation for Scottrade.

**Certain analytical techniques have historically been developed and used mostly in specific domains, with examples including yield optimization used by airlines, survival models used in Life Sciences, Lean principles used in manufacturing, and diversification used in finance. However, these techniques have a strong potential to be used across industries. For example, you might use yield optimization methodologies for online advertising, survival modeling concepts for financial risk analysis and diversification for marketing and supply chain optimization.**

# Part 7: Dan Bennett on Big Data and his focus for this year

Big data is data we already have, the challenge is to put it in a centralized place and to leverage new technologies to analyze it. 

Biggest focus is to get something on the floor and demonstrate real value from it. Also to identify great use cases for connecting content across BUs. Then show that value exposed in products.

One of the biggest reasons to do this is to unlock the potential of the TR as 'one' company. Only by connecting these relationships and allowing our product developers to explore them do we unlock a new set of capabilities that are hard to foresee. 

Within a year there will be a lot of movement on the Open Data side (especially F&R). There is a growing acceptance that there is a lot of power in taking our authorities and letting our customers use that data/metadata. Wants to provide a set of capabilities to the BUs where they can innovate. Wants to provide the platform that those innovations can run on. Believes that it can be done in a very generic and scalable manner. 


