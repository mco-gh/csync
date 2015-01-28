# csync - push/pull a directory tree or file to/from the cloud.

## Use Cases
* Quick and painless propagation of state information across machines.
* Quick and easy populating of new machines (e.g. dotfiles, shell scripts, etc. on new VMs).
* Trivially easy cloud backup and restore.

## Setup
Set the environment variable `CSYNC_BUCKET` to the URL for the bucket in which you wish to store your 
data in the cloud. This URL should take the form `service-provider://bucket-name`, for example:
* `gs://my-sync-bucket`
* `s3://my-sync-bucket`

Before you start using csync, you'll need to create this bucket. This can be done via the `gsutil mb` command
or any other tool you desire for creating a GCS or S3 bucket.

## Syntax

    csync -c -d -h -v [push|pull] [dir-or-file]
    
    -c - base file diffs on checksums (this is slower becaause it calculates checksums on
         every local file). The default is to simply compare file sizes but that obviously
         misses file changes which don't alter the file size.
    -d - Delete any files in the target tree that don't also exist in the source tree.
    -h - display this help text
    -v - product verbose output detailing every file transferred
    
    dir-or-file - Optional aergument specifying the source for a push operation and the destination
                  for a pull operation. If unspecified, this is assumed to be the current directory.
                  
    If the explicitly or implcitly specified path is a directory, the entire directory tree is
    recursively synched with the cloud representation.
    
    Note that cscync compares the local object(s) with the cloud representation(s) and transfers
    data only when the two analogs differ in size (or in content, if the -c option is supplied).
    
## Examples
Move the current directory tree on your master machine to a new virtual machine:

    master$ csync push dir
    new-VM$ csync pull dir
    
Quickly and easily populate your favorite set of shell scripts from the bin directory on your master machine as follows:

    `master$ cd; csync push bin`
    `new-VM$ cd; csync pull bin`
    
Backup the contents of your home directory from your master machine.

    `master$ csync push`
    
Restore the contents of your home directory on your master machine:

    `master$ csync pull`

## Supported Storage Services
* Google Cloud Storage
* Amazon S3

## Dependencies
* The lowest common denominator of shells, /bin/sh.
* The gsutil command, which is included in the Google Cloud SDK (https://cloud.google.com/sdk/).
* A cloud storage account with one of the supported providers.

## Dude, why don't you just use rsync(1)?
* rsync entails a peering relationship between two machines, which can get complicated if you have multiple
  machines you want to keep in synch or if you have limited access to the master (e.g. if it's behind a VPN).
  csync requires only access to a cloud storage service via HTTP and the public internet.
* There's no need to think about a source and a destination, only a source (for a push) or a destination 
  (for a pull). By having an implicit "other side" of the connection (your cloud bucket), I find that csync
  provides a simpler and more natural interface. 
