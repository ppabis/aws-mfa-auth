AWS MFA Script for CLI
======================
This script will help you authenticate to AWS if MFA is enforced. The
credentials given to you by STS will be temporary for the current terminal
session.

Installation
------------
You need to place the script in your $PATH, for example `/usr/local/bin/aws-auth`
or `~/.local/bin/aws-auth`. Also make it executable with `chmod +x aws-auth`. In
order for the script to work you need the following software:

- AWS CLI - look on official AWS website,
- jq - installable with `brew`, `apt` or other package managers,
- dialog - installable with `brew`, on Linux it should be installed by default.

Example usage
------------
```bash
$ . aws-auth # Remember the dot at the beginning
# After you get confirmation, you can use AWS CLI
$ aws s3 ls
# Or even Terraform if you didn't specify AWS credentials in the config
$ terraform plan
```

Configuration
-------------
You need to put your AWS profiles in the file `~/.aws/credentials` - this
includes access key ID and secret access key.

In order for the script to process the profiles, you need to add special
comments:

- `@AuthEntry` - This is the start of a profile that should be processed (required)
- `@MFA:arn:aws:iam::123456789012:mfa/username` - This is the ARN of the MFA
  device that should be used for this profile (required)
- `@Label:EC2Developer` - This is the label that will be used for the MFA
  prompt (optional, will use profile name if not specified)

```ini
# @AuthEntry
# @MFA:arn:aws:iam::123456789012:mfa/username
# @Label:EC2Developer
[EC2Developer]
aws_access_key_id = AKIA123...
aws_secret_access_key = abcd...

# @AuthEntry
# @MFA:arn:aws:iam::123456789012:mfa/iphone
# @Label:RDSAdministrator
[RDSAdmin]
aws_access_key_id = AKIA456...
aws_secret_access_key = efgh...
```

This script is dissected in the blog post:
(https://pabis.eu/blog/2023-12-03-Easy-MFA-AWS-CLI-Terraform.html)