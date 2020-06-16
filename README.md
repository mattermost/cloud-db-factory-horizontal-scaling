# Mattermost Cloud Database Factory Horizontal Scaling

This repository houses the open-source components of Mattermost Cloud Database Factory Horizontal Scaling. This is a microservice with the purpose of checking capacity in existing Aurora RDS Clusters and trigger [Mattermost Cloud Database Factory](https://github.com/mattermost/mattermost-cloud-database-factory) to spin up additional clusters.

## Developing

### Environment Setup
1. Install [Go](https://golang.org/doc/install)
2. Specify the region in your AWS config, e.g. `~/.aws/config`:
```
[profile mm-cloud]
region = us-east-1
```
3. Generate an AWS Access and Secret key pair, then export them in your bash profile:
  ```
  export AWS_ACCESS_KEY_ID=YOURACCESSKEYID
  export AWS_SECRET_ACCESS_KEY=YOURSECRETACCESSKEY
  export AWS_PROFILE=mm-cloud
  ```
4. Clone this repository into your GOPATH (or anywhere if you have Go Modules enabled)
5. Export the following environment variables:
  ```
  export RDSMultitenantDBClusterNamePrefix="The DB cluster name prefix"
  export RDSMultitenantDBClusterTagPurpose="The DB cluster purpose tag"
  export RDSMultitenantDBClusterTagDatabaseType="The DB cluster database type tag"
  export MaxAllowedInstallations="The maximum number of installations before requesting a new RDS cluster"
  export Environment"The environment running e.g Test"
  export DBInstanceType="The type of the DB instance to request from the Database factory"
  export TerraformApply="Whether to request a plan or a Terraform apply from the Database factory"
  export BackupRetentionPeriod="How long to keep database backups for"
  export DatabaseFactoryEndpoint="The endpoint for the database factory"
  export MattermostNotificationsHook="The mattermost hook to use for notifications"
  export MattermostAlertsHook="The mattermost hook to use for alerts"
  export StateStore="The statestore to use for Terraform"
  ```
  
### Building

Simply run the following:

```
$ make build
```

### Running

Run the app with:

```
$ /go/bin/database-factory-horizontal-scaling
```

where dbfactory is an alias for /go/bin/dbfactory
