# Run Book / System Operation Manual

See [`README.md`](README.md) for details of how to use this **Run Book / System Operation Manual** template.

Copyright © 2014-2016 [Skelton Thatcher Consulting](https://skeltonthatcher.com/)

Licenced under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) ![CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/3.0/88x31.png)


## Service or system overview

**Service or system name:** 

### Business overview

> What business need is met by this service or system? What expectations do we have about availability and performance?

_(e.g. Provides reliable automated reconciliation of logistics transactions from the previous 24 hours)_

### Technical overview

> What kind of system is this? Web-connected order processing? Back-end batch system? Internal HTTP-based API? ETL control system?

_(e.g. Internal API for order reconciliation based on Ruby and RabbitMQ, deployed in Docker containers on Kubernetes)_

### Service Level Agreements (SLAs)

> What explicit or implicit expectations are there from users or clients about the availability of the service or system?

_(e.g. Contractual 99.9% service availability outside of the 03:00-05:00 maintenance window)_

### Service owner

> Which team owns and runs this service or system?

_(e.g. The *Sneaky Sharks* team (Bangalore) develops and runs this service: sneaky.sharks@company.com / *#sneaky-sharks* on Slack / Extension 9265)_

### Contributing applications, daemons, services, middleware

> Which distinct software applications, daemons, services, etc. make up the service or system? What external dependencies does it have?

_(e.g. Ruby app + RabbitMQ for source messages + PostgreSQL for reconciled transactions)_

## System characteristics

### Hours of operation

> During what hours does the service or system actually need to operate? Can portions or features of the system be unavailable at times if needed?

#### Hours of operation - core features

_(e.g. 03:00-01:00 GMT+0)_

#### Hours of operation - secondary features

_(e.g. 07:00-23:00 GMT+0)_

### Data and processing flows

> How and where does data flow through the system? What controls or triggers data flows?

_(e.g. mobile requests / scheduled batch jobs / inbound IoT sensor data )_

### Infrastructure and network design

> What servers, containers, schedulers, devices, vLANs, firewalls, etc. are needed?

_(e.g. '10+ Ubuntu 14 VMs on AWS IaaS + 2 AWS Regions + 2 VPCs per Region + Route53')_

### Resilience, Fault Tolerance (FT) and High Availability (HA)

> How is the system resilient to failure? What mechanisms for tolerating faults are implemented? How is the system/service made highly available?

_(e.g. 2 Active-Active data centres across two cities + two or more nodes at each layer)_

### Throttling and partial shutdown

> How can the system be throttled or partially shut down e.g. to avoid flooding other dependent systems? Can the throughput be limited to (say) 100 requests per second? etc. What kind of connection back-off schemes are in place?

#### Throttling and partial shutdown - external requests

_(e.g. Commercial API gateway allows throttling control)_

#### Throttling and partial shutdown - internal components

_(e.g. Exponential backoff on all HTTP-based services + `/health` healthcheck endpoints on all services)_

### Expected traffic and load

> Details of the expected throughput/traffic: call volumes, peak periods, quiet periods. What factors drive the load: bookings, page views, number of items in Basket, etc.

_(e.g. Max: 1000 requests per second with 400 concurrent users - Friday @ 16:00 to Sunday @ 18:00, driven by likelihood of barbecue activity in the neighborhood)_

#### Hot or peak periods

_

#### Warm periods

_

#### Cool or quiet periods

_

### Environmental differences

> What are the main differences between Production/Live and other environments? What kinds of things might therefore not be tested in upstream environments?

_(e.g. Self-signed HTTPS certificates in Pre-Production - certificate expiry may not be detected properly in Production)_

### Tools

> What tools are available to help operate the system?

_(e.g. Use the `queue-cleardown.sh` script to safely cleardown the processing queue nightly)_

## Required resources

> What compute, storage, database, metrics, logging, and scaling resources are needed? What are the minimum and expected maximum sizes (in CPU cores, RAM, GB disk space, GBit/sec, etc.)?

### Required resources - compute

_(e.g. Min: 4 VMs with 2 vCPU each. Max: around 40 VMs)_

### Required resources - storage

_(e.g. Min: 10GB Azure blob storage. Max: around 500GB Azure blob storage)_

### Required resources - database

_(e.g. Min: 500GB Standard Tier RDS. Max: around 2TB Standard Tier RDS)_

### Required resources - metrics

_(e.g. Min: 100 metrics per node per minute. Max: around 6000 metrics per node per minute)_

### Required resources - logging

_(e.g. Min: 60 log lines per node per minute (100KB). Max: around 6000 log lines per node per minute (1MB))_

### Required resources - other

_(e.g. Min: 10 encryption requests per node per minute. Max: around 100 encryption requests per node per minute)_


## Security and access control

### Password and PII security

> What kind of security is in place for passwords and Personally Identifiable Information (PII)? Are the passwords hashed with a strong hash function and salted?

_(e.g. Passwords are hashed with a 10-character salt and SHA265)_

### Ongoing security checks

> How will the system be monitored for security issues?

_(e.g. The ABC tool scans for reported CVE issues and reports via the ABC dashboard)_

## System configuration

### Configuration management

> How is configuration managed for the system?

_(e.g. CloudInit bootstraps the installation of Puppet - Puppet then drives all system and application level configuration except for the XYZ service which is configured via `App.config` files in Subversion)_

### Secrets management

> How are configuration secrets managed?

_(e.g. Secrets are managed with Hashicorp Vault with 3 shards for the master key)_

## System backup and restore

### Backup requirements

> Which parts of the system need to be backed up?

_(e.g. Only the CoreTransactions database in PostgreSQL and the Puppet master database need to be backed up)_

### Backup procedures

> How does backup happen? Is service affected? Should the system be [partially] shut down first?

_(e.g. Backup happens from the read replica - live service is not affected)_

### Restore procedures

> How does restore happen? Is service affected? Should the system be [partially] shut down first?

_(e.g. The Booking service must be switched off before Restore happens otherwise transactions will be lost)_

## Monitoring and alerting

### Log aggregation solution

> What log aggregation & search solution will be used?

_(e.g. The system will use the existng in-house ELK cluster. 2000-6000 messages per minute expected at normal load levels)_

### Log message format

> What kind of log message format will be used? Structured logging with JSON? `log4j` style single-line output?

_(e.g. Log messages will use log4j compatible single-line format with wrapped stack traces)_

### Events and error messages

> What significant events, state transitions and error events may be logged?

_(e.g. IDs 1000-1999: Database events; IDs 2000-2999: message bus events; IDs 3000-3999: user-initiated action events; ...)_

### Metrics

> What significant metrics will be generated?

_(e.g. Usual VM stats (CPU, disk, threads, etc.) + around 200 application technical metrics + around 400 user-level metrics)_

### Health checks

> How is the health of dependencies (components and systems) assessed? How does the system report its own health?

#### Health of dependencies

_(e.g. Use `/health` HTTP endpoint for internal components that expose it. Other systems and external endpoints: typically HTTP 200 but some synthetic checks for some services)_

#### Health of service

_(e.g. Provide `/health` HTTP endpoint: 200 --> basic health, 500 --> bad configuration + `/health/deps` for checking dependencies)_

## Operational tasks

### Deployment

> How is the software deployed? How does roll-back happen?

_(e.g. We use GoCD to coordinate deployments, triggering a Chef run pulling RPMs from the internal yum repo)_

### Batch processing

> What kind of batch processing takes place?

_(e.g. Files are pushed via SFTP to the media server. The system processes up to 100 of these per hour on a `cron` schedule)_

### Power procedures

> What needs to happen when machines are power-cycled?

_(e.g. *** WARNING: we have not investigated this scenario yet! ***)_

### Routine and sanity checks

> What kind of checks need to happen on a regular basis?

_(e.g. All `/health` endpoints should be checked every 60secs plus the synthetic transaction checks run every 5 mins via Pingdom)_

### Troubleshooting

> How should troubleshooting happen? What tools are available?

_(e.g. Use a combination of the `/health` endpoint checks and the `abc-*.sh` scripts for diagnosing typical problems)_

## Maintenance tasks

### Patching

> How should patches be deployed and tested?

#### Normal patch cycle

_(e.g. Use the standard OS patch test cycle together with deployment via Jenkins and Capistrano)_

#### Zero-day vulnerabilities

_(e.g. Use the early-warning notifications from UpGuard plus deployment via Jenkins and Capistrano)_

### Daylight-saving time changes

> Is the software affected by daylight-saving time changes (both client and server)?

_(e.g. Server clocks all set to UTC+0. All date/time data converted to UTC with offset before processing)_

### Data cleardown

> Which data needs to be cleared down? How often? Which tools or scripts control cleardown? 

_(e.g. Use `abc-cleardown.ps1` run nightly to clear down the document cache)_
 
### Log rotation

> Is log rotation needed? How is it controlled? 

_(e.g. The Windows Event Log *ABC Service* is set to a maximum size of 512MB)_

## Failover and Recovery procedures

> What needs to happen when parts of the system are failed over to standby systems? What needs to during recovery? 

### Failover

_

### Recovery

_

### Troubleshooting Failover and Recovery

> What tools or scripts are available to troubleshoot failover and recovery operations?

_(e.g. Start with running `SELECT state__desc FROM sys.database__mirroring__endpoints` on the PRIMARY node and then use the scripts in the *db-failover* Git repo)_

