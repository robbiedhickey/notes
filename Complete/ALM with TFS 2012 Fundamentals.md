## Microsoft ALM Overview

Big takeaway: TFS is more than just version control.

### What is ALM?

* ALM = Application Lifecycle Management
* MS's view of ALM
	* Plan and Track
	* Design
	* Develop
	* Automated Build
	* Testing
	* Test Lab Management
* Ben Day's definition of ALM
	* Not sucking wind when you develop your application
	* Not losing a couple million $$ writing software
		* Not for just large projects
	* Software best practices
		* Development
		* Management
		* Testing

TFS != ALM. Microsoft ALM contains TFS, but also contains Visual Studio. TFS is the glue that makes the ALM process work. 

### Team Foundation Server can help

* TFS streamlines ALM best practices
* TFS alone won't save you
* (Almost everything in software is a people problem)
* TFS will help to:
	* Streamline project management
	* Streamline development best practices
	* Add traceability
	* Gain "situational awareness" (what is everyone working on? etc)

TFS Pieces
* Reporting
* Version Control
* Requirements
* Agile Planning
* Automated Build
* Testing & Defect tracking
* Feedback
* Lab management

### TFS Configuration

* Standard configuration
	* Installed on WS2008/R2 or Windows Server 8
	* SQL Server 2008/R2/2012
	* SQL Server reporting services
	* SharePoint
	* Single or Multiple server
* Basic configuration
	* Installed on Windows 7/8
	* Installs quickly on a single box
	* No SharePoint and no SSRS

### How to connect to TFS

* Visual Studio 2012/13 via Team Explorer
* TFS Web Access - very good for agile planning
* Microsoft Office
	* Excel, Project, PowerPoint, Outlook
	* SharePoint
* Command Line - administration mostly. Also has QA tools.
* Team Explorer Everywhere (TEE) - Linux/OSX
	* Command line
	* Eclipse plugin

### Version Control

* Not SourceSafe
* Enterprise-quality source control
	* Transactional & Atomic check-in
	* Stored in SQL Server
* Features
	* Check-in/Check-out
	* Branch/Merge
	* Label - versions, etc
	* Shelvesets - temporary check-ins, server backed up, not for public consumption
* Check-ins can be associated to work items

### Requirements Management

#### Work Items

* TFS Work Items
	* Scrum: Product Backlog Items
	* Agile: User Stories
* Contain the details for a requirement
	* Including who it's assigned to
* Can be associated to
	* Storyboards
	* QA Test Cases
	* Implementation Tasks
	* Check-ins
	* Builds

#### PowerPoint Storyboards

* A picture is worth 1000 words
* Lightweight requirements
* You don't have to be a genius to use PowerPoint
	* Let the non-technical stakeholders create screen mocks
* Faster to create PowerPoint than a working UI
	* Iterate and tweak until you get it right
	* Build it AFTER there's some agreement. 

### Agile Planning

* Work Items
	* Product Backlog Items, User Stories
	* Tasks
* Product Backlog Management
	* List of everything to do
	* Priority
	* Velocity forecasting
* Sprint Planning
	* Decompose PBIs into Tasks
	* Capacity planning
* Scrum Board
	* Run the daily scrum
	* View and Update Task status
* Burndown Chart
	* How much work is left?
	* Updates in real-time

### Automated Build

* Prevent the "works on my box" problem
* Prove you can build your app
* Makes integration second nature
	* Continuous Integration
* Basis of your release process
	* Version of code are now traceable to builds
	* QA testing and defect tracking against builds rather than guesses

### Testing & Defect Tracking - Testing Types

* Developer Testing
	* Visual Studio 2012
	* Unit Testing - testing at the API level
	* Coded UI Testing - testing a running user interface
* Performance Testing
	* Load Testing your application
	* Web applications or unit tests
* QA Testing - aka "Manual Testing"
	* Microsoft Test Manager (MTM) - separate product
	* Test Plans & Test Cases
	* Exploratory Testing - just playing around in application
	* Defect Tracking
	* Plugs into Lab Management

### Feedback

* Collaborate with your customers and stakeholders
* PowerPoint Storyboarding
	* Lightweight requirements
	* Build it AFTER there's some agreement
* Feedback Manager
	* Request feedback via TFS (rather than email)
	* Track whether you got feedback
	* Track the feedback

#### The Feedback Process

* Create a Feedback Request
	* What do you want feedback on?
		* Web app URL
		* Remote Machine
		* Client Application
	* What questions do you want answered?
	* Request is emailed
* User reviews the application
	* User installs the Feedback Client
	* Can record audio & video
	* Screenshots
	* Text
* You get the Feedback response as a Work Item

### Lab Management

* Deploying builds onto test machines is a pain. For example, dealing with multiple versions of IE
* Managing test machines is a pain
* Lab Management streamlines this with virtualization
	* Hyper-V
	* System Center Virtual Machine Manager
	* Leverages snapshotting
		* You can attach snapshots to bugs, so dev can access VM to recreate
		* Also allows easy rollback
* Lab Management Build type
	* Two builds: Compile build & Deploy build
* Configurable deploy script for each machine in the Environment
* The Steps
	* 1) Run a compilation build
	* 2) Deploy the build
	* 3) Run Coded UI tests (optional)
* After it's deployed..
	* Do nothing
	* Run MTM test cases
	* MTM Exploratory Testing

### Reporting

* TFS gathers a lot of data
* Use this data to
	* Track what's going on
	* Answer questions about quality
	* Answer questions about progress
	* analyze trends
* Stored in relational database
* Stored in a data warehouse
	* SSAS
* SSRS Reports
	* Canned, out of the box reports
	* Create your own
* Microsoft Excel reports
	* Adhoc reports
	* Connect to SSAS cube

## Basic Setup and Configuration

### Pieces of TFS

* TFS Application Tier
	* IIS, Web Services
	* Visual Studio Team Foundation Background Job Agent
* SQL Server
* SSRS
* SSAS
* SharePoint
* Team Build Service
	* Automated build server
	* Controller & Agent
* TFS Proxy Server
	* Version Control Proxy
	* Local version control caching ( great for distributed teams )
	* Nice for teams with slow connections to TFS
* TFS Agents
	* Load Testing Agents
	* Lab Agents
	* Controller & Agent

#### Stuff that bolts on to TFS

* System Center Virtual Machine Manager
	* Enables Lab Management
* Microsoft Project Server
	* Replicate data to/from TFS and Project Server
	* Enterprise Resource Management
	* Do you like Waterfall? Do you have a PMO? You want this.
* PreEmptive Analytics
	* Application self reports errors
	* Integration of ops and development
	* Errors in production -> work items in TFS

### TFS Capacity Planning

* No need to go nuts on hardware
* Install guide recommends, for less than 500 users
	* Dual core, 2.13 GHZ processor
	* 4 GB of RAM
	* 300 GB of disk
	* No SharePoint
* Great fit for VM

I have omitted many sections that seemed more specific to administration...

### Team Projects & Team Project Collections

* Team Project
	* Bucket that contains everything related to a software development effort
	* Has a Name & Process template
* Team Project Collection
	* Collection of team projects
	* Gets its own database
	* Backup & Restore

### Process Templates

* Define the initial structure of a Team Project
	* MS Visual Studio Scrum 2.0
	* MSF for Agile Software Development 6.0
	* MSF for CMMI Process Improvement 6.0

### Permissions & Security

* Security is administered at multiple levels
	* Team Project Collection
	* Team Project
	* Within a Team project
		* Source Control
		* Work Item Tracking
	* SSRS
	* SharePoint

These can be administered through Team Explorer -> Settings -> Security. This will open the TFS web interface. 

## Version Control - Basics

### Why Version Control?

* Collaboration
* Eliminate lost work
* Reproducible Product State
* Reproducible Builds

### TFS Version Control

#### Getting Latest

Team Explorer -> Home -> Source Control Explorer. You will see Folders inside Team project collection. 

Initially you may see "Local Path: Not mapped" and everything is greyed out. You can right-click the project you want and select "Get Latest Version", which will then prompt you to map to a local folder. 

#### TFS Workspaces

Getting latest established a workspace. Must set up mapping between TFS and your computer. Folders in TFS get mapped to folders on your computer. There are two workspace types: Server and Local. 

* Workspace permissions
	* Private, Public, Limited Public
* File time
	* Check-in, Current

#### Workspace Editor

**Workspace mappings can sometimes get messed up**. Typically via operator error. 

There is a workspace dropdown, which includes a link for "Manage Workspaces". Often the result of having multiple workspaces mapped to the same team project. 

The 'Advanced' tab can set the Location, Permission, File time.

#### Workspaces: Server or Local?

* Server
	* Original workspace
	* TFS tracks the state of your workspace
	* Really likes network connectivity
	* If your workspace is huge, use this
	* Compatible with VS2010 and earlier.
* Local
	* New for VS2012
	* Works nicely offline
	* You'll probably want this unless...
		* Your workspace is huge
		* You're a control freak

#### Configure Version Control for a New Project

* Create a folder off of the root
	* call it 'main' or 'trunk'
* Never add any files to the root
	* (just in case you ever want to branch)

In Source Control Explorer, click 'New Folder' icon and name it. Go to 'Pending Changes' and Check In the new folder with comment. You then get a changeset number. 

If you have existing sub-folders, you can click the 'Add Items to Folder' icon. This will outline which items to add, as well as which are ignored by default (bin/*, artifacts, etc).  

When you are configuration your repository, keep it simple. You should be able to get latest, open solution, and compile. If you're not, you're probably doing something wrong.

* Workspace mappings should be simple
	* 99% of the time it should be one entry
	* No crazy copying of DLLs or configs around
	* Does it take you hours everyday to get the app running?
	* Does it take new employees a couple days to get the app running?
	* Constant problems with missing DLLs?

### Version Control Operations

* Pending changes are batched
	* Until you check-in it only exists on your machine
	* (well, everything except labels)
* Check-ins become a Changeset.
	* Changeset number
	* Can be associated to a work item
* Change types:
	* Add, Edit
	* Rename
	* Delete, Undelete
	* Branch, Merge

## Version Control - Beyond the Basics

### Local Workspace: Offline Features

* New for TFS 2012/VS2012
* Things you can do offline:
	* Add / Edit / Rename / Delete / Undo (including undo delete) / Compare
* You can't do offline check-in

Free yourself from the read-only bit!

* Local workspaces don't use the read-only bit
	* In Server Workspaces, any file that is not 'checked out' is read-only.
* Pending Changes window is now smarter with local workspaces
	* Included Changes
	* Excluded Changes
	* Detected Changes
		* These are changes that were made outside of visual studio, so you can use other editors if you so choose. 

### Version Control Locks

* Check-in Lock
	* Anyone can edit locally
	* Can't check-in until you check in or the lock is released
* Check-out lock (unenforceable if you have local workspaces)
	* No one can modify the file (aka "pend a change")
	* Not available if 'Asynchronous Checkout' is enabled
* Can be overridden if you have the UnlockOther permission. 

You can lock a file by right-clicking the file in the Source Control Explorer -> Advanced -> Lock. Seems like you can also lock folders. Unlock follows the same procedure. 

### Shelving and Shelvesets

* Temporary check-ins
* No one sees it until they look for it
	* Does not commit to the main repository
* Used for
	* Code Reviews
	* Gated Check-in Builds
	* My Work's Suspend & Resume
	* Sharing your work with someone else
	* Backing up your work

To shelve, create some pending changes. Right-click on the solution -> Source Control -> Shelve Pending Changes -> Give shelf a name. They are attached to your username, so just make sure it is descriptive to you. 

To unshelve, go to Source Control Explorer. Select any folder in your project -> File -> Source Control -> Find -> Find Shelvesets -> Pick your shelveset -> Right-click -> Unshelve.

### Branching & Merging

* Branch = work on multiple versions of a similar code base simultaneously
	* Multiple versions
	* Feature development isolation
* Merge = move changes between these versions

#### Best Practices

* Don't branch
* If you must branch, have a good reason (merges can be a time sink)
* Understand what a "release" means. 

If you are not deterred, see the [Visual Studio TFS Branching and Merging Guide](http://vsarbranchingguide.codeplex.com)

#### Demo

To branch, go to Source Control Explorer. Right-click folder -> Branching and Merging -> Branch -> give branch a name. Now you will have a bunch of pending changes that are marked as 'branch'. A branch is a pending change just like everything else. 

To merge, go to Source Control Explorer. Right-click folder -> Branching and Merging -> Merge -> Set source and target branch, pick changesets or all -> Next -> Finish. You will now have pending changes of type merge/edit. If they look good to you, you can then check-in the pending changes. 

To resolve conflicts, follow the merge steps. If a conflict is encountered, you will be presented with the option of AutoMerge, 'Merge Changes In Merge Tool' or 'Keep Target Branch Version'. Most often you will want to use the mergetool. 

Mergetool is a standard Source | Target with a Result pane at the bottom. When you are done resolving, click the 'Accept Merge' button. Then you can commit your changes. 

To visualize your branches, right-click the folder in Source Control Explorer -> Branching and Merging -> View Hierarchy. 

### Server-Side Settings

#### Team Project Collection

Go to Team Explorer -> Settings -> Team Project Collection. 

Allows you to set various things, including what types of files allow merging and multiple check out. Some things are not mergeable (binary files, etc). You can also set your default workspace settings, the default is Local in VS2012. 

#### Source Control

Go to Team Explorer -> Settings -> Source Control Settings. 

Checkout-out settings. Recommends setting 'Enable multiple check out' to true and 'Enable get latest on check out' to false. We want to control when we get latest. 

Check-in Policy. Can enforce, for example, work item association. Also a changeset comment policy. 

#### Permissions

Go to Source Control Explorer -> Right-click project -> Advanced -> Security. Gives you access control groups for modification. 

## Work Item Basics

### What is a Work Item?

* Configurable document type in TFS
* Title, Status, Assigned To, Create Date, and other fields
* Has a customizable user interface
* Tracks history of edits
* Can be queried via Work Item Queries
* Scoped to a Team Project 

### Ways to Edit Work Items

* Visual Studio using Team Explorer
* TFS Web Access
* Microsoft Excel
* Microsoft Project
* 3rd Party Tools & Plugins
* The TFS APIs

### Work Item Types 

#### MSVS Scrum 2.0 Template

* Product Backlog Item
	* "Requirement" in scrum
	* aka PBI
* Impediment
	* Something that is causing a delay or blockage
* Task
	* Something to do related to a PBI
* Bug
	* A defect or issue found in the application
* Test Case
	* List of steps a tester should take to validate a PBI

#### MSF for Agile 6.0 Template

* User Story
	* "Requirement"
	* Product Backlog Item or PBI
* Issue
	* Something that is causing a delay or blockage
* Task 
	* Something to do related to a PBI
* Bug
	* A defect or issue found in the application
* Test Case
	* List of steps a tester should take to validate a PBI

### Work Item Links

* Define Relationships between work items
* Parent & Child Hierarchies
* Predecessor & Successor
	* Dependencies
	* Microsoft Project
* Test Case, Tested By, Tests, Shared Steps
* Related
* Storyboard

Can use 'New Linked Work Item' or 'Link To' icons to associate work items. It will require you to define a link type. Note: Changesets association is done via work item linking. Links can be in between more than just other items.

### Areas & Iterations

* Areas
	* Subdivide requirements & tasks into buckets
	* By Team? By Feature? Other?
* Iterations
	* Subdivide by time
	* Sprint, Iteration
* Configurable Hierarchies
	* Maximum 14 levels
	* Form "paths"
	* If path text is longer than 256 chars, it won't work in MS Project

### Work Item Queries

* Find and organize work items
* Think "SQL queries for work items"
* Result types
	* Flat List
	* Work Items and Direct Links
	* Tree of Work Items (full hierarchy)

Out of the box you get a hand-full of queries by default (depending on the process template selected). 

* Product Backlog
* Feedback
* Current Sprint
	* Blocked Tasks
	* Open Impediments
	* Sprint Backlog
	* Test Cases
	* Unfinished Work
	* Work in Progress

Any of these are editable, by the right-click menu. You can change the criteria as well as the result type, including the columns you want displayed. TFS also gives you contextual variables such as @Project. 

You can also create your own queries, and save them for later reference. These will show up under the My Queries node. You can also 'Add to Favorites' for quick access or 'Add to my team favorites' to share. When you add them to team favorites, these queries will now show up on the web access for all members.   

## Agile and Scrum Planning Tools

### Teams in TFS

When a project gets large, we tend to start splitting into teams. Teams are now a first-class feature in TFS. 

* New feature in 2012
* Grouping of people within a team project
* Teams have
	* Members
	* Areas
	* Iterations
	* Work Item Query Favorites
	* Team Picture
	* Alerts
* Basically, their own project plan. 

### Create a team

Need to go to web access. Top right-hand corner (administer server). Notice that by default a team is already created. You can then create new teams and begin adding members.  

You can also select which areas and iterations are relevant for your team. A team also gets their own area created for them by default. 

Alerts are also configurable at a team level. Very configurable, for example, email build notifications, etc. 

Security can also be managed at the team level. 

### Scrum & Agile Planning Tools

If you are running an agile or scrum team, you're going to have some variation of:

* Product Backlog
	* List of all "desirements" for the product
	* Desires + Requirements
	* Ordered by priority
* List of Sprints or Iterations
	* Timeboxes in which team does work
	* Start date & end date
	* Should always be the same number of days
* Sprint Backlog
	* List of PBIs for the sprint
	* List of Tasks to make the PBIs "done"

TFS comes with a number of tools for these components:

#### Product Backlog Manager

* Quickly add PBIs
* Order the backlog using drag & drop
* Use Velocity to estimate delivery dates

This is also where you would estimate story points / effort if you so choose. 

Protip: You can enable forecasting, and see how much would get done given different velocity estimates. The grid will show you which iteration/sprint the PBI would be completed based on the velocity entered. 

#### Sprint Backlog Manager

* Easily create tasks for a sprint
* View team & individual capacity

From the Product Backlog Manager, you can drag and drop PBIs onto sprints in the left-hand navigation and the sprint path will automatically be updated. You can do the same to assign things back to the backlog.

Once this is done you can click on the sprint and view only the PBIs for that iteration. This is where you will add child tasks to flesh out requirements and estimate time. Note that the sub-tasks also have to be associated to an iteration. 

This screen will also include team capacity estimates, which are derived from the capacity tab. Lets you set capacity (as working hours, usually 6) and the activity category (development, testing, design, etc). Can also include days off here, which will automatically update the capacity estimates. 

#### Scrum board

* View status of PBIs and Tasks
* Update remaining work & status using drag & drop

Accessible via the 'Board' tab. Visual indicator of backlog items (To Do -> In Progress -> Done) which include story points and time estimates. Can also filter the board by person. You can drag and drop these 'cards' in between statuses. It also gives you quick access to update the number of hours worked which will automatically update the global estimates. 

#### Burndown Chart

* Graphs remaining work
* Visual indicator of whether you're on schedule

Burndown chart is in the top right-hand corner of many screens. Graph of remaining work against time remaining in iteration. Includes an ideal burndown slope to see your progress, and allows you to assess where you are at any time during the sprint. Requires everyone to be diligent in updating their hours. 

## Work Item Customization

N/A for now.

## Automated Build - Basics

N/A for now.

## Automated Builds - Beyond the Basics

N/A for now.