# auto-snapshot

Automate EC2 volume snapshots. Inspired by [ec2-automate-backup](https://github.com/colinbjohnson/aws-missing-tools/tree/master/ec2-automate-backup) but with the following differences:

- doesn't depend on AWS cli tools
- timestamps stored in human readable form
- name and description tags include volume's friendly name, for easy identification

=======

<img src="http://porjo.github.io/auto-snapshot/snapshots.png"></img>

## Usage

[Setup an EC2 instance with an appropriate IAM role to allow snapshots to be created/deleted.](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)

```
$ ./auto-snapshot --help
Usage of ./auto-snapshot:
  -k int
    	Purge snapshot after this many days. Zero value means never purge
  -p	Enable purging of snapshots (default true)
  -region string
    	AWS region to use
  -tagPrefix string
    	String to prefix to tag name, description (default "auto-snap")
  -tags value
    	Select EBS volumes using these tag keys e.g. 'Daily-Backup'. Tag values should be == 'true'
```

Typical usage:

Tag each EBS volume that you would like to include in the automatic snapshot with a unique tag key e.g. `Backup-Daily` and set the tag's value to `true`. Run `auto-snapshot` and specify the tag key that you used, together with the number of days snapshots should be kept:

```
./auto-snapshot -region="ap-southeast-2" -tags="Backup-Daily" -k=3
```

## Building

Get a [precompiled binary](https://github.com/porjo/auto-snapshot/releases) for Linux or compile from source using Go build tools

```
go get -u github.com/porjo/auto-snapshot
$GOPATH/bin/auto-snapshot
```
