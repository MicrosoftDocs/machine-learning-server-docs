---

# required metadata
title: "DeployR Server, Grid & Database High Availability"
description: "How to Configure High Availability HA for DeployR Server, Grid & Database"
keywords: ""
author: "j-martens"
manager: "Paulette.McKay"
ms.date: "05/06/2016"
ms.topic: "article"
ms.prod: "microsoft-r"
ms.service: ""
ms.assetid: ""

# optional metadata
ROBOTS: ""
audience: ""
ms.devlang: ""
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.technology: "deployr"
ms.custom: ""

---

# DeployR Server and Grid High Availability

## Server High Availability

When discussing high-availability in the context of server systems, clustered Java Web applications are a common architectural solution. However, due to the non-serializable socket connections maintained per R session by the DeployR server, full distributed session management across servers in a cluster is not possible, as it depends on serializable session state.

There are however alternative deployment options that can be adopted to improve server availability and overall uptime. The relative merits of these deployment options are discussed in the following sections.

### Primary-Standby Deployment

This deployment model while simple can be highly effective. Your application makes use of a `primary` server and a `standby` server that share a single instance of the DeployR database. By default, your application talks only to the `primary` server. If the `primary` server becomes unresponsive at runtime, your application detects this condition and transitions away from the unresponsive `primary` by redirecting all traffic to the `standby` server.

The following steps detail how to configure this type of deployment:

1.  **DeployR Database Setup**

    -   Install a single, standalone instance of the DeployR database.
    -   This single database instance will be used by the `primary` and `standby` servers, as detailed next.

2.  **Primary Server Setup**

    -   Install a dedicated DeployR server and designate it as your `primary`.
    -   Configure the MongoDB database endpoint for the primary using deployr.groovy.
    -   Set the primary `deployr.server.policy.override.web.context` property using deployr.groovy.

3.  **Standby Server Setup**

    -   Install a dedicated DeployR server and designate it as your `standby`.
    -   Configure the MongoDB database endpoint for the standby using deployr.groovy.
    -   Set the standby `deployr.server.policy.override.web.context` property using deployr.groovy.

4.  **DeployR Grid Setup**

    -   Using either the `primary` or `standby` server administration console:
    -   Add details for each grid node you have provisioned for your deployment.

At this point your `Primary-Standby Deployment` is fully configured. As the `primary` and `standby` servers in your deployment are sharing a single instance of the DeployR database your servers are also sharing:

1.  A single DeployR repository where all R scripts, models, etc. are managed.
2.  A single DeployR grid.

While this can be an effective deployment model please take careful note of the following:

1.  Failover is not automatic. Your application must detect when the `primary` becomes unresponsive and then actively take steps to re-route all traffic to the `standby`.

2.  A single grid means zombie tasks (initiated by a `primary` that then fails) can leak grid resources (memory and/or processor cycles) that can not be reclaimed by the `standby` when it takes over active management of the grid. This type of resource impairment can lead to performance degradation over time.

Potentially a better deployment model would isolate failures to ensure resource impairment by zombie tasks would not continue to impact on overall availability or responsiveness of the system. This forms the motivation for the next deployment model to be described, `Load Balanced Deployment`.

### Load Balanced Deployment

While multiple DeployR servers can not participate in a classic cluster configuration with automatic failover it is possible to deploy multiple servers behind a HTTP load balancer to distribute load and even adapt to server failures at runtime.

The following steps detail how to configure this type of deployment:

1.  **Server Setup \* N**

    -   Install two or more DeployR servers.
    -   Each server should be configured to use its own dedicated database.
    -   Each server should be configured to use its own dedicated set of grid nodes.

2.  **Load Balancer Setup**

    -   Install your preferred HTTP load balancer.
    -   Configure the load balancer to enforce sticky HTTP sessions.
    -   Register the IP endpoint of each DeployR server with your load balancer.

At this point your `Load Balanced Deployment` is fully configured. In the event of a server failure all other servers in the cluster will continue functioning unaffected. However, given each server in your deployment operates independently of the other servers there is one important implication:

Your R scripts, models, etc. must be manually replicated across the DeployR repositories associated with each of your servers.

## Grid High Availability

The DeployR grid is designed to deliver high-availability at runtime, supporting both redundancy and failover. The server monitors the health off all resources on the grid at runtime. If an individual slot on the grid or even an entire grid node becomes unresponsive, the server automatically contracts the grid, bypassing those failing resources. If failing resources once again become responsive the server automatically expands the grid, to include the newly responsive grid resources.

In this way, the DeployR grid can be said to be `self-healing`, capable of automatically adjusting to system failures and recoveries at runtime, ensuring maximum uptime and responsiveness.

> **RBroker High Availability!** The `7.4.0` release of the [RBroker Framework](deployr-rbroker-framework.md) introduced a new fault-tolerant implementation of the `PooledTaskBroker`. Tasks executing on the new PooledTaskBroker that fail due to grid failures at runtime are automatically detected and re-executed without requiring client application intervention.

<!-- This was for MongoDB & DeployR 8.0.0
## Database High Availability

We highly recommended that serious consideration be given to deploying [MongoDB replica sets](http://www.mongodb.org/) for MongoDB in DeployR production environments. Through the use of replica sets, the MongoDB database is capable of supporting data synchronization across multiple database servers. These replica sets provide data redundancy and increase data availability.

**Deployment Recommendations and Notes:**

-   A replica set must have an odd number of members.

-   In production deployments, each member should be hosted on a separate machine.

-   The default MongoDB port for this release of DeployR is `8003`.

## Database HA Configuration Example

The following example will guide you through the setup and deployment of a three member MongoDB replica set for DeployR on Linux. A three member replica set is the smallest replica set one can deploy, but is typically capable of providing enough redundancy to survive network partitions as well as other system failures.

The following assumptions were made in this example:

-   An instance of DeployR server was installed with one instance of the DeployR MongoDB database, either co-located or remote from the DeployR server.

-   This MongoDB instance is running on port `8003`, the default MongoDB port for DeployR Enterprise.

-   This MongoDB instance will be the primary database in the replica set.

-   We are installing MongoDB on a supported Linux operating system.

-   `<INSTALL_DIR>` is the full path to the directory into which DeployR 8.0.0 was installed.

### Stop the DeployR Server

1.  Log onto the DeployR server host machine.

2.  Stop the Tomcat process on that machine. At the prompt, type:

        <INSTALL_DIR>/tomcat/tomcat7.sh stop

### Creating Key Files

Create a keyFile on the primary host machine and set the permissions on this file. The location of that file will be: `<INSTALL_DIR>/mongo/mongodb-keyfile`, where `<INSTALL_DIR>` is the directory into which you installed DeployR.

       cd <INSTALL_DIR>/mongo
       openssl rand -base64 741 > mongodb-keyfile
       chmod 600 mongodb-keyfile

>[!NOTE]
>You will need to transfer a copy of this file to the same location on each of the secondary machines.

### Set Up Two Secondary Databases

The existing MongoDB instance will act as the primary database in this replica set. To complete a three member replica set, install two more standalone instances of the MongoDB database to act as the secondary databases.

For each standalone MongoDB instance, do the following:

1.  Log on to the machine that will host the secondary MongoDB instance.

2.  [Install MongoDB database](deployr-installing-configuring.md#deployr-install-with-remote-database).

    >Follow the installation instructions provided with DeployR since this ensures the database is properly initialized for use with DeployR. (This is `option 2` in the automated [Linux installation](deployr-installing-configuring.md#installing-on-linux).)

3.  Change the default password for this secondary MongoDB instance to the password defined in `DeployR MongoDB Configuration Override` section of the `<INSTALL_DIR>/deployr/deployr.groovy` file on the primary host machine.

    >[!NOTE]
	>The default password and user for the DeployR MongoDB database is:  
	>   User = `deployr`       Password = `changeme`
    

    1.  In a console window, start MongoDB. At the prompt, type:

            <INSTALL-DIR>/mongo/mongo/bin/mongo admin -u deployr -p changeme --port 8003

    2.  Change the password. At the prompt, type:

            db.changeUserPassword("deployr", "<MongoDB_password_on_primary>")
            # add deployr as a user
            use deployr
            db.createUser(
            {
              user: "deployr",
              pwd: "<MongoDB_password_on_primary>",
              roles: [ "readWrite" ]
            }
            )
            exit

4.  Shutdown the MongoDB instance. At the prompt, type:

        <INSTALL_DIR>/mongo/mongod.sh stop

5.  Transfer a copy of the `mongodb-keyfile` file you created on the primary machine to the following location on the secondary host machine, `<INSTALL_DIR>/mongo/mongodb-keyfile`.

6.  Edit the MongoDB instance configuration file on the machine hosting that member.

    1.  Open the file `<INSTALL_DIR>/mongo/mongod.conf`.

    2.  Add the following lines to the configuration file:

            replSet = deployrReplSet 
            keyFile = <INSTALL_DIR>/mongo/mongodb-keyfile

    3.  Save these changes and exit the file.
         

7.  Restart the MongoDB instance. At the prompt, type:

        <INSTALL_DIR>/mongo/mongod.sh start

8.  Repeat these steps for the other secondary member.

### Reconfigure Primary Database

1.  Log onto the DeployR MongoDB primary host machine.

2.  Shutdown the MongoDB instance. At the prompt, type:

        <INSTALL_DIR>/mongo/mongod.sh stop

3.  Edit the MongoDB instance configuration file.

    1.  Open the file `<INSTALL_DIR>/mongo/mongod.conf`.

    2.  Add the following lines to the configuration file:

            replSet = deployrReplSet 
            keyFile = <INSTALL_DIR>/mongo/mongodb-keyfile

    3.  Save these changes and exit the file.
         

4.  Restart the MongoDB instance. At the prompt, type:

        <INSTALL_DIR>/mongo/mongod.sh start

### Configure the Replica Set

1.  Log onto the DeployR MongoDB primary host machine.

2.  Start MongoDB console. At the prompt, type:

        <INSTALL_DIR>/mongo/mongo/bin/mongo admin -u deployr -p <new_mongo_password> --port 8003

3.  Initialize the MongoDB replica set configuration. In the MongoDB console, type:

        rs.initiate()

    >[!NOTE]
>Please allow time for this command to complete.

    Once complete, the console prompt will be prefixed as such: `deployrReplSet:PRIMARY>`

4.  Set the primary IP address on replica set configuration. In the MongoDB console, type the following at the prompt:

        rscfg = rs.config()
        rscfg.members[0].host = “<primary-db-ip>:8003”
        rs.reconfig(rscfg)

    Where `<primary-db-ip>` should be replaced with the IP address for the host machine running the MongoDB primary database.

5.  Add the secondary databases to the replica set configuration. In the MongoDB console, type the following at the prompt:

        rs.add(“<secondary-one-db-ip>:8003”)
        rs.add(“<secondary-two-db-ip>:8003”)

    Where `<secondary-one-db-ip>` and `<secondary-two-db-ip>` should be replaced with the IP address for the host machines running the secondary databases respectively.

    Verify the configuration. In the MongoDB console, type the following at the prompt:

        rs.status()

    In the output from the `rs_status()` command, verify that there are one PRIMARY and two SECONDARY members.

    See the `stateStr` values in the replica set configuration. As an example:

        deployrReplSet:PRIMARY> rs.status()
        {
         "set" : "deployrReplSet",
         "date" : ISODate("2013-10-18T19:28:30Z"),
         "myState" : 1,
         "members" : [
             {
                 "_id" : 0,
                 "name" : " <primary-db-ip> : 8003",
                 "health" : 1,
                 "state" : 1,
                 "stateStr" : "PRIMARY",
                 "uptime" : 615,
                 "optime" : Timestamp(1382124446, 1),
                 "optimeDate" : ISODate("2013-10-18T19:27:26Z"),
                 "self" : true
             },
             {
                 "_id" : 1,
                 "name" : " <secondary-one-db-ip> : 8003",
                 "health" : 1,
                 "state" : 2,
                 "stateStr" : "SECONDARY",
                 "uptime" : 82,
                 "optime" : Timestamp(1382124446, 1),
                 "optimeDate" : ISODate("2013-10-18T19:27:26Z"),
                 "lastHeartbeat" : ISODate("2013-10-18T19:28:28Z"),
                 "lastHeartbeatRecv" : ISODate("2013-10-18T19:28:29Z"),
                 "pingMs" : 0,
                 "syncingTo" : " <primary-db-ip> : 8003"
             },
             {
                 "_id" : 2,
                 "name" : " <secondary-two-db-ip> : 8003",
                 "health" : 1,
                 "state" : 2,
                 "stateStr" : "SECONDARY",
                 "uptime" : 64,
                 "optime" : Timestamp(1382124446, 1),
                 "optimeDate" : ISODate("2013-10-18T19:27:26Z"),
                 "lastHeartbeat" : ISODate("2013-10-18T19:28:30Z"),
                 "lastHeartbeatRecv" : ISODate("2013-10-18T19:28:30Z"),
                 "pingMs" : 10,
                 "syncingTo" : " <primary-db-ip> : 8003"
             }
         ],
         "ok" : 1
        }

### Configure DeployR to Use Replica Set

1.  Log onto the DeployR server host machine.

2.  Open the DeployR external configuration file, `<INSTALL_DIR>/deployr/deployr.groovy`.

3.  Locate the file section named DeployR MongoDB Configuration Override.

4.  Uncomment the replicaSet property and substitute the IP addresses for your primary (`<primary-db-ip>`) and two secondary databases (`<secondary-one-db-ip>` and `<secondary-two-db-ip>`).

    Verify that the file looks similar with the correct IP addresses.

         grails {
             mongo {
                 databaseName = "deployr"
                 username = "deployr"
                 password = "<mongo_password>"
                 host = "localhost"
                 port = 8003
                 replicaSet = [ "<primary-db-ip>:8003",
                                "<secondary-one-db-ip>:8003",
                                "<secondary-two-db-ip>:8003"]
                 options {
                     connectionsPerHost = 100
                     autoConnectRetry = true
                     connectTimeout = 300
                 }
             }
         }

5.  Save and exit the external configuration file.

### Restart DeployR Server

Once the DeployR server restarts, the server will now be using a highly available, redundant database store.

1.  Log onto the DeployR server host machine.

2.  Start the Tomcat process on that machine. At the prompt, type:

        <INSTALL_DIR>/tomcat/tomcat7.sh start

-->
