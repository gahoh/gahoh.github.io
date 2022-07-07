
---
title: "Release Note"
description: "Contains information about the changes in the current GridDB v5.0 version"
date: 2020-01-28T00:34:41+09:00
draft: false
weight: 41

---

## --- Introductiona ---

This document describes about release notes of GridDB Enterprise Edition (These products are represented simply as GridDB in this document).
The description includes information, such as product requirements and manuals, required to use GridDB.
Please read this document before using GridDB.

The contents of this document are as follows.

1.  Introduction
1.  Overview and Changes
1.  Function list
1.  Product requirements
1.  Installation packages
1.  GridDB manuals
1.  Dealing with problems
1.  Important notes and limitations
1.  Notices
1.  History of the changes in previous versions
1.  Trademarks
1.  Supplementary explanation

  

## --- Overview and Changes ---

## Overview of GridDB

GridDB is a key-value datastore which can handle, store and search, an enormous amount of data at high speed and safely.
 GridDB has the following features.

- Speeding up data operations by in-memory processing
- Handling a large amount of data with its scale-out model
- High reliability with data persistency by storage devices, data replication and transaction management.
- Easy operation control by autonomous management of node addition and deletion
- Utilizing SQL through JDBC/ODBC interface as a database access method

  
GridDB V4.5 and later now provides a simple product architecture which consists of:

  |                              |                        |
  |------------------------------|------------------------|
  | GridDB Enterprise Edition (EE) | NewSQL type database product  featuring a NoSQL type database provided with an SQL interface.|


For checking functions provided by GridDB, see "[GridDB Release Notes](../1.md_release_notes/md_release_notes.md)".

For starting to use GridDB, see "[GridDB Quickstart Guide](../2.md_quickstart_guide/md_quickstart_guide.md)" .

See Fixlist.pdf in the installation media for the correction records including bug fixes.

## Changes in V5.0
The data management unit has been revised to enhance the performance for massive data. A summary of changes in V5.0 is given as below:

**<Performance>**

- Improving core scaling

  By restarting a node, concurrency (/dataStore/concurrency in the node definition file) can now be changed.

- Faster scan and data deletion

  By setting the string **#unique** as hint information for data affinity, it is now possible to occupy blocks on a per container (table) basis to place data.

- Reduction of the disk I/O load when performing a checkpoint

  It is now possible to write logs in several smaller batches during a checkpoint. The number of batches (splits) can be modified by /checkpoint/partialCheckpointInterval in the node definition file. It is 10 by default. While increasing the settings value can decrease the amount of writing to a checkpoint log file at a time, this may increase the recovery time during a startup.

**<Operation functions>**

- Database conversion tool
 
  Databases in V4 format have been converted into V5 format.


**<Supplementary explanation>**
- Compatibility with previous versions
  - API

    A V5 cluster can be operated using the API for V4. Note that when a discontinued functionality is used, the cluster generates an error. Note also that a V5 cluster cannot be operated using the API for V3 and earlier.

  - Database

    For databases in V4 database format, convert them into V5 database format using the conversion tool. For databases in V3 database format, retrieve data using gs_export for V3 and store them in  V5 database using gs_import for V5.

  - Cluster

    A cluster cannot be configured if both V4 and V5 nodes are present.

  - Settings file

    The settings file (gs_node.json/gs_cluster.json) for V4 can also be used as the settings file for V5. The parameters added in V5 are enabled using the default value. The parameters that have been discontinued in V5 are ignored.

    - Parameters added in V5 (gs_node.json)
       - /dataStore/transactionLogPath

         Specify the full or relative path to the location directory for a transaction log file. The default value is txnlog.

       - /checkpoint/partialCheckpointInterval

         Specify the number of splits in write processing of block management information to checkpoint log files during a checkpoint. The default value is 10.

    - Parameters that have been discontinued in V5
       - /dataStore/autoExpire
       - /dataStore/dbFilePathList
       - /dataStore/storeWarmStart
       - /checkpoint/checkpointMemoryLimit
       - /checkpoint/useParallelMode

- Change of the output parameter name for statistical information

  The following output parameter names for GridDB performance and statistical information that can be retrieved using the operation command gs_stat have been changed.
  
  | (V4 and earlier)<br>output parameter name     | (V5.0)<br>output parameter name  | description |
  |----------------------------|-----------------------|-----------------------|
  | /performance<br>/checkpointFileSize          | /performance<br>/dataFileSize          | data file size (in bytes) |
  | /performance<br>/checkpointFileUsageRate     | /performance<br>/dataFileUsageRate     | data file usage |
  | /performance<br>/checkpointFileAllocateSize  | /performance<br>/dataFileAllocateSize  | total size of blocks allocated to data files (in bytes) |
  | -                                        | /performance<br>/checkpointWrite       | Write count to the date file by checkpoint processing |
  | -                                        | /performance<br>/checkpointWriteCompressTime        | Compression time of write data to the data file by checkpointing process (ms) |
  | /checkpoint<br>/archiveLog                   | -                     | discontinued |  
  | /performance<br>/checkpointMemory            | -                     | discontinued |
  | /performance<br>/checkpointMemoryLimit       | -                     | discontinued |
  | /performance<br>/expirationDetail/autoExpire | -                     | discontinued |
  
- Changes in supported operating systems

  Ubuntu Server 2004 is now supported. Starting with the current version, Red Hat Enterprise Linux 7.6, 7.7, 7.8, 8.1, and 8.2, and CentOS 7.6, 7.7, 7.8, 8.1, and 8.2 are no longer supported.

- Functionalities that have been discontinued in V5
  - Hash index
  - Row expiry release
  - Timeseries compression
  - Trigger function
  - Long-term archive
  - Functionality to work with RDB for export and import

[Memo]

- For changes in previous versions, see [History of the changes in previous versions](#change_logs).

## --- Function list ---

The functions of GridDB EE are as follows.

| Title             | Features                         | Enterprise Edition |
|------------|--------------------------------|----------|
| Cluster           | Cluster configuration            | ✓ |
|            | Distributed data management      | ✓ |
|            | Replication                      | ✓ |
| Data Management   | Collection                       | ✓ |
|            | Timeseries container             | ✓ |
|            | Index                            | ✓ |
|            | Affinity                         | ✓ |
|            | Table Partitioning               | ✓ |
| Query Language    | TQL                              | ✓ |
|            | SQL                              | ✓ |
| API               | NoSQL Interface (Java/C)         | ✓ |
|            | NewSQL Interface (JDBC/ODBC)     | ✓ |
| Operation Control | Backup                           | ✓ |
|             | Export/Import                    | ✓ |
|             | Integrated operation control GUI | ✓ |
|             | Operating commands               | ✓ |
|             | LDAP authentication            | ✓ |
|             | SSL communication             | ✓ |
| Time series data  | Expiry release                   | ✓ |

      

# --- Product requirements ---

The product requirements of GridDB are as follows. The requirements are different for the server and the client.

## Server: Platform to run the database server

|             |                                 |
|-------------|---------------------------------|
| CPU | x64 processor, 2.0GHz (minimum) or higher |
| Memory | 1.0GB (minimum) or more               |
| Disk Space | This is the requirement only for installation. Separate space is required for database and log files. The size depends on the application (at least 1.0GB). |
| OS          | Red Hat Enterprise Linux 7.9 (64 bit)<br>Red Hat Enterprise Linux 8.3 (64 bit)<br>CentOS 7.9 (64 bit)<br>CentOS 8.3 (64 bit)<br>Ubuntu Server 20.04 (64 bit) |

Following specifications or higher performance hardware is recommended for the server.

|            |              |
|------------|--------------|
| CPU                 | x64 processor |
| Operating frequency | 2.66GHz      |
| Number of CPUs      | 2            |
| Number of Cores     | 4            |
| Memory              | 32GB         |

The following LDAP server is supported for LDAP authentication.

  |                      |                         |
  |----------------------|-------------------------|
  | LDAP server      | OpenLDAP 2.4<BR>Active Directory Schema Version 87 (Windows Server 2016)|


The following software is required to perform SSL communication.

  |                      |                         |
  |----------------------|-------------------------|
  | OpenSSL                 | OpenSSL 1.1.1 |

**Required specifications for operation control commands**

- Software

  |                          |                   |
  |--------------------------|-------------------|
  | Python                   | Python 3.6        |

- Software (during SSL communication)

  |                          |                   |
  |--------------------------|-------------------|
  | OpenSSL                 | OpenSSL 1.1.1 |

## Client: Platform to run the developed application

**Required specifications for the application using Java API, C API, JDBC, Python, Node.js or Go**

- Platform

  |             |                                 |
  |-------------|---------------------------------|
  | CPU | x64 processor, 2.0GHz (minimum) or higher |
  | Memory | 1.0GB (minimum) or more               |
  | OS          | Red Hat Enterprise Linux 7.9 (64bit)<br>Red Hat Enterprise Linux 8.3 (64 bit)<br>CentOS 7.9 (64 bit)<br>CentOS 8.3 (64 bit)<br>Ubuntu Server 20.04 (64bit)<br>Windows Server 2016 / Windows 10 (64 bit) *JDBC driver only|

- Software

  |                          |                   |
  |--------------------------|-------------------|
  | Java                  | Oracle JDK 8<br>Oracle JDK 11<br>OpenJDK 8<br>OpenJDK 11 |
  | Python                   | Python 3.6        |
  | Node.js                  | Node.js 12        |
  | Go                       | Go 1.9            |

  

**Required specifications for the application using ODBC or Windows C API**

- Platform

  |             |                                 |
  |-------------|---------------------------------|
  | CPU | x64 processor, 2.0GHz (minimum) or higher |
  | Memory | 1.0GB (minimum) or more               |
  | OS | Windows Server 2012 R2/2016<br>Windows 10 (64bit) |

  

**Required specifications for operation control commands**

- Software

  |                          |                   |
  |--------------------------|-------------------|
  | Python                   | Python 3.6        |

  The following software is required to perform SSL communication.

  |                          |                   |
  |--------------------------|-------------------|
  | OpenSSL                 | OpenSSL 1.1.1 |





## Platform to use the integrated operation control GUI (gs_admin)

- Platform

  |             |                                 |
  |-------------|---------------------------------|
  | CPU | x64 processor, 2.0GHz (minimum) or higher |
  | Memory | 1.0GB (minimum) or more               |
  | OS          | Red Hat Enterprise Linux 7.9 (64 bit)<br>Red Hat Enterprise Linux 8.3<br>CentOS 7.9 (64 bit)<br>CentOS 8.3 (64 bit)<br>Ubuntu Server 20.04 (64 bit) |

- Software

  |                      |                         |
  |----------------------|-------------------------|
  | Java                  | Oracle JDK 8<br>Oracle JDK 11<br>OpenJDK 8<br>OpenJDK 11 |
  | Browser | Internet Explorer 11.0  |

  

## Platform to use the Web API

- Platform

  |             |                                 |
  |-------------|---------------------------------|
  | CPU | x64 processor, 2.0GHz (minimum) or higher |
  | Memory | 1.0GB (minimum) or more               |
  | OS          | Red Hat Enterprise Linux 7.9 (64 bit)<br>Red Hat Enterprise Linux 8.3 (64 bit)<br>CentOS 7.9 (64 bit)<br>CentOS 8.3 (64 bit)<br>Ubuntu Server 20.04 (64 bit) |

- Software

  |                      |                         |
  |----------------------|-------------------------|
  | Java                 | Oracle JDK 8<br>Oracle JDK 11<br>OpenJDK 8<br>OpenJDK 11 |

  The following software is required to perform SSL communication.

  |                      |                         |
  |----------------------|-------------------------|
  | OpenSSL                 | OpenSSL 1.1.1 |

## Operation platform for the Dockerfile sample

- Platform

  |             |                                 |
  |-------------|---------------------------------|
  | CPU         | x64 processor, 2.0GHz (minimum) or higher |
  | Memory      | 1.0GB (minimum) or more               |
  | OS          | Red Hat Enterprise Linux 8.3 (64bit)<br>CentOS 8.3 (64bit) |


- Software

  |                      |                         |
  |----------------------|-------------------------|
  | Docker               | Docker CE 20.10.10      |


## User Guide for GridDB Template for Ansible Operation Platform

- Platform

  |             |                                 |
  |-------------|---------------------------------|
  | OS          | CentOS 8 (64bit)            |

- Software

  |                      |                         |
  |----------------------|-------------------------|
  | Ansible              | Ansible 2.11.6          |
  | Python               | Python 3.6              |

  

## --- Installation packages ---

There are 9 RPM packages.


| Package                 | File name                                     | Description    |
|----------------|----------------|---------------|
| Server package          | griddb-ee-server      | This package contains GridDB server module and operation commands for server start-up, backup/restore database, etc. |
| Client package          | griddb-ee-client      | This package contains operation control commands, export/import and cluster operation control command interpreter (gs_sh). |
| Web UI package| griddb-ee-webui | This package contains the integrated operations management GUI (gs_admin). |
| Web API package         | griddb-ee-webapi      | This package contains a Web API. |
| C library package       | griddb-ee-c-lib      | This package contains a C header file (/usr/include/gridstore.h) and a library (/usr/lib64/libgridstore.so, libgridstore_advanced.so). |
| Java library package    | griddb-ee-java-lib   | This package contains a Java library (/usr/share/java/gridstore.jar, gridstore-advanced.jar, gridstore-jdbc.jar). |
| Python library package  | griddb-ee-python-lib | This package contains a Python package (griddb_python).  |
| Node.js library package | griddb-ee-nodejs-lib | This package contains a Node.js module (griddb_node).  |
| Go library package      | griddb-ee-go-lib     | This package contains a Go package (griddb/go_client).  |


To find out the content of the installation media and the installation method, see the following manual. "[GridDB Quickstart Guide](../2.md_quickstart_guide/md_quickstart_guide.md)".

  

## --- GridDB manuals ---

GridDB manuals are as follows.


| Manual | Target | Description                                                      |
|-------------------------------|----------------------------|------------------------------------------------------------|
| GridDB Quick Start Guide | Users using GridDB for the first time | This manual describes overview, functions and basic operations of GridDB.  |
| GridDB Database Administrators Guide | Infrastructure designer<br />Operations manager | This manual describes the physical design and operation of the database. |
| GridDB Programming Guide | Application developer | This manual explains how to use Java and C language APIs using example programs such as data registration and search. |
| GridDB SQL Tuning Guide | Application developer | This manual describes SQL tuning procedures and SQL optimization rules. |

- Reference

  Targets are infrastructure designers, operators, and application developers.

  | Manual | Description                            |
  |-------------------------------------------|---------------------------------|
  | GridDB Features Reference | This manual describes features and functions of GridDB. |
  | GridDB Operation Tools Reference | This manual describes how to use each operation control command, the export/import tools, the integrated operation control GUI (gs_admin) and the cluster operation control command interpreter (gs_sh).|
  | GridDB Error Codes | This manual describes GridDB error codes and their causes and approaches.|
  | User Guide for GridDB Monitoring Template for Zabbix | This manual explains how to use Zabbix settings and templates for monitoring GridDB. |
  | User Guide for GridDB Dockerfile sample | This manual explains how to use the GridDB Dockerfile sample. |
  | User Guide for GridDB Template for Ansible | This manual explains how to use templates that support the construction of GridDB using Ansible. |
  | GridDB TQL Reference<br />GridDB Java API Reference<br />GridDB C API Reference<br />GridDB SQL Reference<br />GridDB JDBC driver guide<br />GridDB Advanced Edition ODBC driver guide<br />GridDB Web API Reference<br />GridDB Python API Reference<br />GridDB Node.js API Reference<br />GridDB Go API Reference | This manual describes APIs and query language for the application development. |

  

## --- Dealing with problems ---

If there are any problems in using GridDB, see the following manual. "[GridDB Features Reference](../3.md_reference_feature/md_reference_feature.md)" "[GridDB Error Codes](../6.md_error_code/md_error_code.md)"

  

## ---  Important notes and limitationsa ---

This section provides important notes and limitations that should be confirmed and understood before using the product.

  

## Important notes

| No | Title | Description |
|----|------|------|
| 1 | Compatibility (database) | In GridDB V5.0 or later, databases created in GridDB V4 or earlier are not available. <br />For databases in V4 database format, convert them into V5 database format using the conversion tool. <br />For database files for GridDB V3 or earlier, data migration by the export/import tools is required. |
| 2 | Cluster with different major versions of GridDB nodes is not permitted | The cluster must be consisted of only the same major version of GridDB nodes in V4.1.1 or later. <br />Note that different major versions of GridDB nodes are not mixed in the network. <br />Further in GridDB V3.5 or earlier versions, different major and minor versions of GridDB nodes cannot be mixed in a cluster. |
| 3 | Compatibility (Zip output format in exporting) | It is not possible to use zip files exported before V2.7 in V2.7 or newer version. |
| 4 | Column type | In a SQL interface, the following NoSQL interface-specific data types cannot be used as table column types. <br />- Geometry (GEOMETRY) type<br />- Array type |
| 5 | SQL | To handle data types and sequences in a query flexibly, there are the cases where their error is not detected. |  
| 6 | Compatibility (nodes in a cluster, API and a cluster) | - Version compatibility among nodes in a cluster<br />A rolling grade cannot be used to migrate from V4 to V5. <br /><br />- Version compatibility of API and a cluster<br />A V5 cluster can be operated using the API for V4. <br />Note that when a discontinued functionality is used, the cluster generates an error. <br />Note also that a V5 cluster cannot be operated using the API for V3 and earlier. |

  

## Limitations


| No | Title | Description |
|----|------|------|
| S30007 | Database names and user names are case-sensitive in some operations | In the operations for the creation, uppercase and lowercase characters are identified as the same. But in operations for the connection, reference and deletion, the names are treated as case-sensitive. <br /><br /><br />* After creating a database "dbTest", the database named "DBTEST" or "dbtest" cannot be created. <br />\-&gt; In creating a database, the database names different only in uppercase and lowercase characters are identified as the same name. <br /><br />* The database "dbTest" is not deleted by specifying "DBTEST" or "dbtest". It is required to specify "dbTest" in order to delete the database. <br />\-&gt; In deleting a database, the database names different in uppercase and lowercase characters are identified as different names. |
| S30009 | Empty geometry values can not be registered | An error occurs when registering empty geometry value.<br />Examples of empty geometry data:<br />POINT(EMPTY), LINESTRING(EMPTY), POLYGON(EMPTY), etc. |
| S30014 | Table partitioning cannot be performed on the table to which the node affinity is set | Creating a table by specifying both the node affinity and the partitioning causes an error. Example) CREATE TABLE table1@affinity(id integer) PARTITION BY HASH(id) PARTITIONS 5 |
| I40004 | If columns are added with putContainer of the NoSQL API, the NULL value is set in the corresponding columns of existing rows. | The empty value is set until V4.0.3. It will become selectable from the empty value and the NULL value in the future version. |
| T30004 | In case of failure, gs_admin may not be able to get container information | When a part of partitions cannot be accessed for a database failure, gs_admin cannot get container information. Even in that case, gs_sh can get the container information on the normal partitions. |
| T30006 | Clusters, databases and containers which name contains any special characters ('-', '.', '/', '=') cannot be handled by the Web API | When specifying a cluster name, a database name or a container causes an error. |

<a id="change_logs"></a>
## ---  History of the changes in previous versionsa ---
## Changes in V4.6
A summary of the function enhancements in V4.6 is as below:

**\<Functions for development\>**

- Renaming a column using SQL

  The following SQL is supported:
  - ALTER TABLE *table name* RENAME COLUMN *column name before renaming* TO *column name after renaming*;

- Unifying the Communication Edition (CE) and SQL implementation.

  Unified SQL implementation for GridDB CE and EE. Through elimination of the differences in features and performance, migration from CE to EE is made easier. No functional changes are No functional changes have been made since V4.5 EE except for the renaming of columns by SQL.


**\<Operation functions\>**

- Enhancing the SSL function
  
  Server certification verification by the client is supported. It verifies the signed certificate with the trusted certificate authority.


**\<Supplementary explanation\>**
- Changes in supported operating systems

  Red Hat Enterprise Linux 7.9/8.2/8.3 and CentOS 7.9/8.2/8.3 are supported. This version and later versions no longer support Red Hat Enterprise Linux 6 and CentOS 6.


## Changes in V4.5
GridDB Enterprise V4.5 is released. A summary of enhancements made available in V4.5 is as given below: The abstract of the function enhancements is as below.

**\<Operation functions\>**

- LDAP authentication

  LDAP authentication is supported as external authentication, which enables to centrally manage authentication information.

- Securing communications paths through SSL.
  
  SSL has been enabled for communication between the GridDB cluster and the client.

- Enhancement of the integrated operations management GUI (gs_admin)

  Web containers have been included in gs_admin. This has eliminated the need to prepare a Web server in advance in order to execute the service.

- Enhancement of the cluster operation control command interpreter (gs_sh)
  
  Subcommands for registering/deleting a row, searching for a container, synchronizing cluster node definitions, and displaying/running history are newly added.

  The format of a history file (gssh_history) has been changed for enhancing the history function.
  If the alert "bad history file syntax" is displayed while running the gs_sh command, delete the history file.


**\<Supplementary explanation\>**
- Support for new OS versions

  Red Hat Enterprise Linux 7.8  and 8.1 and CentOS 7.8 and 8.1 are supported.

- Support for new Java versions

  Oracle JDK 11 and RedHat OpenJDK 11 are supported.

- Adding and deleting installation packages

  The following packages are added.

  - Web UI package (griddb-ee-webui)

  The following package is deleted.

  - NewSQL package (griddb-ee-newsql)

  The following package names have been changed to the ones shown in parentheses:
  - server package (griddb-ee-server)
  - client package (griddb-ee-client)
  - Web API package (griddb-ee-webapi)
  - C library package (griddb-ee-c_lib)
  - Java library package (griddb-ee-java_lib)
  - Python library package (griddb-ee-python_lib)
  - Node.js library package (griddb-ee-nodejs_lib)
  - Go library package (griddb-ee-go_lib)
  
  Upgrade installation from the V4.3 or earlier packages is not available. Uninstall those packages and reinstall.

## Changes in V4.3.4

A summary of the function enhancements in V4.3.4 is as below:

**\<Functions for development\>**

- Expansion of SQL functions (AE only)

  Supports the following clauses:
  - WINDOW function: LAG (gets the line before the current line), LEAD (gets the line after the current line)

  The following aggregate functions are available as analytic functions with the OVER clause:
  - AVG (average value), COUNT (number of rows), MAX (maximum value), MIN (minimum value), SUM (total value)/TOTAL (total value), STDDEV_SAMP (sample standard deviation), STDDEV (sample standard deviation)/ STDDEV0 (sample standard deviation), STDDEV_POP (population standard deviation), VAR_SAMP (sample variance), VARIANCE (sample variance)/VARIANCE0 (sample variance), VAR_POP (population variance)



## Changes in V4.3.2

A summary of enhancements made available in V4.3.2 is as given below:

**\<Functions for development\>**

- Expansion of SQL functions

  Supports the following clauses:
  - OVER clause    
  
    Splits and sorts query results. Used with the WINDOW function.

  The following functions are supported.
  - Aggregate functions: STDDEV_SAMP (sample standard deviation), STDDEV0 (sample standard deviation), STDDEV_POP (population standard deviation), VAR_SAMP (sample variance), VARIANCE0 (sample variance), VAR_POP (population variance), MEDIAN (median)
  - WINDOW function: ROW_NUMBER (sequential value)

  The specifications of the following functions are changed:
  - Aggregate function: STDDEV   

     Changed to return the sample standard deviation.   
     Previously, the population standard deviation was returned.
  - Aggregate function: VARIANCE   

     Changed to return the sample standard deviation.   
     Previously, the population variance was returned


## Changes in V4.3.1

A summary of enhancements made available in V4.3.1 is as given below:

**\<Functions for development\>**

- Supporting SQL I/F and table partitioning of data affinity function

  Data affinity can be set using SQL.
  It can also be set when creating a partitioned table.

- Expansion of SQL functions

  The following functions are supported.
  - Arithmetic function: HEX_TO_DEC (hexadecimal string)

  The specifications of the following functions are changed:
  - Arithmetic functions: TRUNC (truncate to the specified number of digits)
    
    Changed to return the result in LONG type when an integer is specified in the argument,  
    whereas previously, DOUBLE type was returned.

  - Datetime function: STRFTIME (convert TIMESTAMP to string)

    %W (week number) was added to the format string

- JDBC enhancement
  
  The following methods are supported:
  - PreparedStatement#setBinaryStream(int parameterIndex, InputStream x)
  - PreparedStatement#setBinaryStream(int parameterIndex, InputStream x, int length)
  - PreparedStatement#setBinaryStream(int parameterIndex, InputStream x, long length)
  - PreparedStatement#setBlob(int parameterIndex, InputStream inputStream)
  - PreparedStatement#setBlob(int parameterIndex, InputStream inputStream, long length)

- ODBC enhancement
  
  The timeout time can be set during SQL execution.

**\<Supplementary explanation\>**

- Adding supported Operating Systems
  
  Red Hat Enterprise Linux 7.7 / CentOS 7.7 are now supported.

- User Guide for GridDB Template for Ansible
  
  This manual provides GridDB templates for Ansible that support initial configuration, acquiring settings, changing settings, and adding nodes.



## Changes in V4.3

A summary of enhancements made available in V4.3 is as given below:

**\<Scalability\>**

- Increasing block size

  The block size, specified at the initial setting when creating a database, has been expanded up to 32MB.
  By specifying a large block size, the amount of data managed by a node increases.
  Furthermore, scanning performance of large scale data is expected to improve.

- Splitting checkpoint files

  The checkpoint file can be split and located in multiple directories, allowing the amount of data managed by a node to increase.
  allowing the amount of data managed by a node to increase. By reducing the load of each disk, overall performance is also expected to improve.
  

- Network configuration which separates the external and internal communication

  The external and internal communication can be separated by assigning different network interfaces to external communication, which is between the client and the nodes, and to internal communication, which is between the nodes.
   Network load can be distributed.

**\<Performance\>**

- Composite index

  An index set on multiple columns can be created. Search performance is expected to improve in a case when combining multiple column values increases data selectivity.
  

**\<Functions for development\>**

- Composite row key

  Row key can be set on multiple columns. which is useful when Row key of multiple columns specifies data uniquely.
   Data design becomes more simple since surrogate keys (substitute keys) are not required.
  

- Specifying time zone

  Time zone is added to the client properties. Time zone argument is added to some TQL and SQL date functions.
  

- Expansion of SQL functions

  The following functions are supported.
  - Aggregate functions: STDDEV (standard deviation), VARIANCE (variance)
  - Arithmetic functions: LOG (logarithm), SQRT (square root), TRUNC (truncate to the specified number of digits)
  - String function: TRANSLATE (string replacement)
  - Date/time functions: STRFTIME (convert TIMESTAMP to character string), MAKE_TIMESTAMP (generate TIMESTAMP), TIMESTAMP_TRUNC (truncate to specified granularity)

**\<Operation functions\>**

- Improvement in access rights
  READ (read only) has been added as the type of permission for general users to access the database.
  The conventional access right was ALL (full access) only. Access rights to multiple general users can be assigned to one database.
  

The following points are changed from GridDB V4.3.

- Change in the time string format

  The format of the time string output from the server and a client has been changed,
  except when the time zone is UTC.

  Example) When the time zone is JST:
  - V4.2 or earlier
    ```
    2019-10-01T12:04:37.726+0900
    ```
  - V4.3
    ```
    2019-10-01T12:04:37.726+09:00
    ```

  The output result and logs of operation tools are also in the same format.

- Partitioning key restrictions

  In the default setting of V4.3, the column specified as the partitioning key must be the one with the primary key.
  

  To specify a column other than the one with the primary key, like in the versions prior to V4.3, add the following settings to the cluster definition file.
  
  ```
      :
    "sql": {
        :
      "partitioningRowkeyConstraint": false
    }
      :
  ```

- Maximum partition size

  The upper limit of the partition size corresponding to the block size has been changed as follows.

  - For the block size of 64KB: changed from about 64TB to about 4TB
  - For the block size of 1MB, changed from about 1PB to about 64TB

- Change in gs_sh specification

  For grantacl/revokeacl subcommands, specifying the access right is required.

- Removed restriction

  The following restrictions has been removed.

  | No | Title | Description |
  |----|------|------|
  | S30010 | Swap files for SQL intermediate results don't shrink | When the memory size used for SQL exceeds the upper limit(sql.storeMemoryLimit in gs_node.json), the contents of memory is dumped into the swap files. These swap files are created on the directory specified by sql.storeSwapFilePath (the file name is "swap_N.dat"). <br />They are deleted when the server is shut down, but in the current version the file size is not reduced while the server is running. <br /><br />If there is a query that puts a large amount of intermediate result into the swap file, even after the query is finished, the disk space where the swap file is stored may be consumed by the query. <br />Please take the following actions beforehand. <br />* Before starting the real operations, check the swap file size by executing the query preliminarily and estimate the required disk space. <br />* By sql.storeSwapFilePath, allocate the swap files in a device or a partition where the database files are not stored. |

## Changes in V4.2.5

The following changes have been made in V4.2.5:

- Dockerfile sample

  Samples for creating and starting GridDB containers that run on Docker CE and Microsoft Azure Container Instance (ACI) are provided.
  - Dockerfile and configuration file to generate Docker container image of GridDB server node and client node.
  - See User Guide for GridDB Dockerfile sample for how to build the container image and how to run the container.

## Changes in V4.2

A summary of the function enhancements in V4.2 is as below:

**\<Performance\>**

- Using multiple indexes

  Plans to use multiple indexes attached to tables sufficiently are generated and executed.

  Moreover, you can control patterns of using indexes changing the describing order of conditions.

- Controlling buffers for search

  The amount of used buffers are controlled monitoring the amount of swap read of a job or a task of queries.

  It reduces the performance degradation of a register process caused by the swap out of analysis queries.

**\<Functions for failure analysis and performance analysis\>**

- Designating application names

  The application name (applicationName) is added to the connection property.

  It is output to the column values of a certain metatable, event logs, and client trace logs.

  It is useful for specifying the application with problems.

- Client trace log output (JDBC) 

  You can have a JDBC application output trace logs including a function name and arguments.

  You can analyze a failure and debug efficiently using detail application logs.

- Slow query log

  If the processing time of a query (SQL) exceeds a threshold, the query information is output to event logs at the end of processing.

  It is useful for specifying a query that takes a lot of time more than expected.

  The threshold is set to the node definition file. You can change it online.

- Output the current process list during execution

  A metatable is added to obtain the NoSQL or NewSQL process information during execution.

  It is useful for specifying what process uses the resources of a node and an OS.

  Moreover, the sub command of gs_sh is added.
  - showsql : Displaying SQL processes during execution
  - showevent : Displaying events during execution
  - showconnection : Displaying connections

- Canceling a query during execution

  The sub command for canceling a query that is not terminated for a long time manually is added to gs_sh.
  - killsql : Canceling a SQL process during execution

  You can confirm a resource ID to cancel a query using the sub command showsql.

- Displaying an execution plan

  The sub command displaying an execution plan for performance analysis of a query is added to gs_sh.
  - getplantxt : Obtaining a SQL analysis result (Text format)
  - getplanjson : Obtaining a SQL analysis result (JSON format)
  - gettaskplan : Obtaining a detail SQL analysis result

- Improving the GridDB monitoring template for Zabbix

  The SQL statistics information, a monitoring item, etc. for log analysis are added to the GridDB monitoring template for Zabbix that is provided as a sample for monitoring a GridDB cluster.

- Improving the log display command (gs_logs)

  The following options are added to make it easy to analyse event logs.
  - The option to output result as a CSV format (--csv)
  - The option for performance trace analysis (--tracestats)
  - The option for slow query log analysis (--slowlogs)

- Improving the command for obtaining cluster information (gs_stat)

  The following option to make it easy to analyze performance information is added.
  - The option to output result as a CSV format (--csv)

**\<Functions for development\>**

- View

  The function for defining a view and syntaxes for creating and deleting it are added. The view list can be referred in a metatable.

  The view is a object similar to a container, however, it does not contain real data. When you execute query including a view, the result of evaluating SELECT sentences defined at creating it is returned.

  Only the reference operation (SELECT) is valid for a view.

- Web API v2

  The APIs such as creating and deleting a container, obtaining a container list, and executing a TQL sentence are added.

  Moreover, you can activate and stop it as a standalone service (griddb-webapi.)

  Please refer the GridDB Web API Reference for details.

**\<Programming language\>**

- Node.js

  You can use Node.js as a programming language to develop an application.

- Go

  You can use Go as a programming language to develop an application.

- C (For Windows environment)

  You can use the C Client library on the Windows environment.

- Improving the exception handling of C API

  Parameters related to errors are added to a part of APIs.

  You can use the parameters to obtain information without analyzing strings.

**\<Supplementary explanation\>**

- Specifying a partition arrangement

  You can use the operational command (gs_goalconf) to specify a partition arrangement. It is mainly used for the rolling upgrade.

- Adding supported Operating Systems

  You can use GridDB on Red Hat Enterprise Linux 7.6, 6.10 / CentOS 7.6, 6.10.


The following points are changed from GridDB V4.2.

- Adding install packages

  The following packages are added.
  - Web API package (griddb-ee-webapi)
  - Node.js library package (griddb-ee-nodejs_lib)
  - Go library package (griddb-ee-go_lib)

- Deleting an install package

  The following package is deleted. If you update version, please uninstall it in advance.

  - Document package (griddb-ee-docs)

- Node definition file (gs_node.json)

  The parameters of the node definition file are changed as below.

  - Changing a parameter value

    The default value of the max memory size used by operators in the SQL process is changed.

    - /sql/workMemoryLimit 128MB -> 32MB

- Deleting SQL hint clauses

  The following hint clauses are deleted.
  - MaxDegreeOfParallelism(Maximum number of processing threads for one query)
  - DistributedPolicy(Selection of distributed planning policy)

- Adding partition status (V4.2.1)

  The following status has been added: In V4.2.0 and earlier, this status was displayed as NORMAL.
  - INITIAL: Initial state not joined the cluster configuration

## Changes in V4.1

A summary of enhancements made available in V4.1 is as given below:

**\<Data Management functions\>**

- Expanding the upper limit of the number of columns

  The upper limit of the number of columns that can be handled on containers (tables).

  It was 1024; however, it becomes 1024-32000 from V4.1. (It depends on the setting of a block size or the column type of a container.)

- Improving a dynamic schema change (column addition)

  It becomes possible to access a container (table) when a column is added to the end of it.

  It is not possible to access it when the schema of it is changed in versions earlier than V4.1.
  Moreover, the process of adding columns to the end of a container becomes faster.

- Expanding an expiry release function

  It becomes possible to set an expiry release in the case that a container (table) the type of which is a partitioned collection is used.
  (However, the following conditions must be satisfied.) The partitioning type is interval or interval-hash The partitioning key is a TIMESTAMP type

  The expiry release can be set only if a container is a timeseries type in versions earlier than V4.1.

  A "partition expiry release" is added as an expiry release type.
  Data is released in a data partition unit.
  Data is released in a row unit in the existing row expiry release.

  The partition expiry release can be set in the case that a container (table) the partitioning type of which is interval or interval-hash.
  When the retention time has passed from the upper bound time of the data range in the data partition, all the rows in the data partition are released.

**\<Operation functions\>**

- A long-term archive function

  It is possible to save data released by the expiry release function to an external file (archive file) for reuse before been deleted from GridDB automatically.

  It enables users to keep the data size of DB and save old data.
  The saved archive file can be filtered by a period and a container name to refer needed files. Moreover, it can be reimported in GridDB.

- Enhancing the backup function

  Following functions are provided to execute an online backup of GridDB and recover to the latest status collaborating with the fast copy function of a storage or a snapshot solution.

    - Checkpoint control function (Pause and restart a checkpoint)
    - Transaction log backup function (Enhancing the existing function)

- Enhancing the rolling upgrade function

  The script of rolling upgrade is provided to make it easy to design an operation method and execute.


**\<Programming language\>**

- Improving the performance of the Python client

  The registration/search performance of the Python client is improved.


**\<Supplementary explanation\>**

- Adding supported Operating Systems

  Red Hat Enterprise Linux 7.5 / CentOS 7.5 are now supported.

  

The following points are changed in GridDB V4.1.

- NoSQL I/F

  The default value of failoverTimeout is changed from 60 second to 120 second. The value is a timeout time used in the case of a node failure.

- Clients try to connect a normal node automatically in the time.

  Template names of definition files ".tmpl" is added to the end of definition file names in the install directory.

  - Example:
    - Node definition file: /usr/griddb/conf/gs_node.json.tmpl
    - Cluster definition file: /usr/griddb/conf/gs_cluster.json.tmpl
    - User definition file: /usr/griddb/conf/password.tmpl

- Node definition file (gs_node.json)

  The parameters of the node definition file are changed as below.
    
  - Changing a parameter value

    The default value of the number of concurrent threads for processing SQLs is changed.

    - /sql/concurrency 4 (existing value is 5)
    
  - Adding a new parameter

    It is possible to set the value that represents the automatic deletion function of expiry released data is enabled or not.

    - /dataStore/autoExpire Set the automatic deletion of expiry released data

    false (automatic deletion disabled) is set as a default value in the template file of a node definition.
    It is regarded as true (automatic deletion enabled) if it is not specified.

    Users using the node definition file before V4.1 need not to modify it.
    The function is not changed (automatic deletion enabled).


- SQL specification

  The following option for the expiry release function that is used in executing create table is added.

    - expiration_type The expiry release type (PARTITION/ROW) is set. The default value is PARTITION.

  The default value is changed from ROW to PARTITION.

  If you use SQL sentences creating timeseries containers with the expiry release setting in the version before V4.1, please add the specification of the type (ROW) of an expiry release to SQL sentences. expiration_type='ROW'

- gs_export/gs_import

  The property file path is changed as below.

  - /var/lib/gridstore/expimp/conf/gs_expimp.properties

    (previous) /usr/griddb/prop/gs_expimp.properties

- gs_admin/gs_sh

  The way to display the column value of the TIMESTAMP type in the TQL/SQL function is unified as below.

  - Time format: ISO8601
  - Timezone: UTC
    - Example) The TIMESTAMP type column value

      2018-11-07T12:30:00.417Z

  

## Changes in V4.0.3

The following changes have been made in V4.0.3:

- Enhancement of table partitioning

  New types of table partitioning are added (Interval and Interval-Hash).
  By the variety of the types, it is possible to select an appropriate partitioning according to the nature of data or application, and then a large amount of data is stored and managed efficiently.

  - Interval
    - The table is partitioned by a specified interval range.

  - Interval-Hash
    - This is a combination of "Interval" and "Hash".
      The table is partitioned by "Interval" initially, and then it is further partitioned by hashing.

  

## Changes in V4.0.2

The following changes have been made in V4.0.2:

- Expansion of Python client

  Python client is enhanced to improve usability and to work with a data analysis library.

  - Interface improvement

    Such as registration and acquisition can be handled without being aware of data types in rows.
    Data specified in Python list is implicitly converted to each column type of the row.

    - Ex.) Registering a row into a container.

      Container.put([1, "value", False])
        
- Pandas linkage

  Python client can work with a data analysis library "Pandas".
  Data acquired by Python client can be analyzed in Pandas and analysis data in Pandas can be registered into a GridDB cluster.
        
  - Function enhancement

    - Array type : Array type can be used in rows
    - NULL value : Rows containing NULL value can be handled
    - Named index : Named indices can be created and deleted
    - Partial execution mode: As an option for acquiring query results, partial execution mode can be specified.

  Old Python clients for previous versions become obsolete.
  Use new Python client in V4.0.2 or later.
  And Python2 becomes unsupported as a running environment of the Python client.


- Placement information display function of containers (tables)

  Containers (tables) in a GridDB cluster are automatically distributed and placed on each node.
  It is possible to check where each container (table) is placed by using an operation control tool or other methods.

  This function can be used for checking containers placed on each node when DB sizes are not balanced among nodes, and taking a backup of nodes which have particular containers.

- Enhancement of Java API exception handling

  Map of parameters related to the error is added to the GSException Class.
  By using this map, a part of information can be acquired without parsing.

- Enhancement of automatic recovery function

  Node will be automatically restarted even if the user or the OOM killer kills the node process.

- Adding supported Operating Systems

  Red Hat Enterprise Linux 7.4 / CentOS 7.4 are now supported.

  

## Changes in V4.0

Following features are changed in GridDB V4.0.

- Expansion of characters used for names

  The characters which are allowed to be used in cluster names, database names, container names, etc. are increased.
  Special characters (hyphen '-', dot '.', slash '/', equal '=') are added to the characters in prior versions.

  By this expansion, it is easier to cooperate between GridDB and other NoSQL database systems, because object names of other systems can also be used in GridDB without rewriting the names in more cases.

- Enhancement of interoperability between NoSQL and NewSQL

  In addition to the enhancement in V3.5, TQL through the NoSQL interface can access partitioned tables created through the NewSQL interface.
  For partitioned tables, either TQL or SQL can be selected to handle the tables depending on the application like containers.

- Rolling upgrade

  From V4.0, for upgrading a minor version or patching, it is possible to upgrade the product while the cluster is running.

- Ensuring database and API compatibility

  From V4.0, database and API backward compatibility is ensured in the same major version.

- Support of the Python programming language

  Python can be used as a programming language for application developments.
  The APIs for Python2 and Python3 are newly provided.

- SQL operations through the Web API

  Execution of a SQL SELECT statement through the Web API is newly available.


  

## --- Notices ---

Basic support contract is required for the use of perpetual license.
 Please make the basic support contract at the same time of purchasing this product.
 It is not possible to contract the support service after the purchasing.


  

## --- Trademarks ---

- GridDB is a registered trademark of Toshiba Digital Solutions Corporation in Japan.
- Oracle and Java are registered trademarks of Oracle Corporation, its subsidiaries or affiliated companies in USA and other countries. There are the cases where the company or product name in this file is a trademark or a registered trademark.
- Linux is a trademark of Linus Torvalds.
- Red Hat is a registered trademark or a trademark of Red Hat, Inc. in USA and other countries.
- Zabbix is a registered trademark of Zabbix SIA.
- Docker is a registered trademark of Docker Inc. in the United States and other countries.
- Microsoft and Azure are registered trademarks of Microsoft Corporation in the United States and other countries.
- Ansible is a registered trademark or trademark of Red Hat, Inc. in the United States and other countries.
- Each other product name is a trademark or registered trademark of the owner.

  

## --- Supplementary explanationa ---

The information about software and its license used by GridDB is included in Readme_en.txt bundled in the server package and the install media.


Contact the representative of the basic support service if there are any inquiries to use this product.

  

                    Copyright Toshiba Digital Solutions Corporation 2017-2022
