# csync

Simple command to push/pull a directory tree or file to/from the cloud.

## Use Cases
* Sharing state information across machines.
* Quickly populating information on new machines (e.g. VMs).
* Backup/restore to/from the cloud.

## Setup
Set the environment variable `CSYNC_BUCKET` to the URL for the bucket in which you wish to store your 
data in the cloud. This URL should take the form `service-provider://bucket-name`, for example:
* `gs://my-sync-bucket`
* `s3://my-sync-bucket`

Before you start using csync, you'll need to create this bucket. This can be done via the `gsutil mb` command
or any other tool you desire for creating a GCS or S3 bucket.

## Syntax

    csync -d -h -v [push|pull] [dir-or-file]
    
    -d - Delete any files in the target tree that don't also exist in the source tree.
    -h - display this help text
    -v - product verbose output detailing every file transferred
    
    `dir-or-file` - Optional aergument specifying the source for a push operation and the destination
                  for a pull operation. If unspecified, this is assumed to be the current directory.
                  
    For a push (pull) operation, The explicitly or implcitly specified directory or file is recursively 
    copied to (from) the cloud. 
    
## Examples
Quickly and easily populate your favorite set of shell scripts from the bin directory on your master machine as follows:
    master$ `cd; cscync push bin`
    
    newVM$ `cd; csync pull bin`
    
Backup the contents of your home directory from your master machine.
    master$ `cd; csync push`
    
Restore the contents of your home directory on your master machine:
    master$ `cd; csync pull`

## Supported Storage Services
* Google Cloud Storage
* Amazon S3

## Dependencies
* The lowest common denominator of shells, /bin/sh.
* The gsutil command, which is included in the Google Cloud SDK (https://cloud.google.com/sdk/).
