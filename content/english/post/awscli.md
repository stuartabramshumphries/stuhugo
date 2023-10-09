---
title: "AWS cli quickstart"
date: 2023-01-02T19:59:02+01:00
Description: ""
Tags: []
Categories: []
DisableComments: false
---
### AWS CLI Quickstart
what is aws cli?  
essentially a command line python tool you can use to query/create/change state of all things in aws.  
pretty cool for performing ad-hoc commands.  
installing:  
% pip3 install awscli  
.
.
check installed ok. 
% aws --version  
aws-cli/1.17.8 Python/3.8.1 Darwin/19.3.0 botocore/1.14.8  
%  
% aws help or aws command help  
% complete -C aws_completer aws   <== add this to your .bashrc  
^-^ stuart@stuartah ~ $ aws s3 (tab-tab here shows possible options below)  
cp ls mb mv presign rb rm sync website configure  
% aws configure  
AWS Access Key ID [None]: **************  
AWS Secret Access Key [None]: *******************  
Default region name [None]: eu-west2  
Default output format [None]: json  
%  
you can also add credentials as env variables - e.g. export AWS_[ACCESS/SECRET]_KEY=...  

here we're adding the credentials to allow connectivity.  
instead of running aws configure you can also edit ~/.aws/config and credentials:  

% ls -l  
total 9  
-rw------- 1 stuart stuart 42 Apr  2 17:03 config  
-rw------- 1 stuart stuart 89 Apr  2 17:03 credentials  
% cat config  
[default]  
region = eu-west2  
output = json  
% cat credentials  
[default]  
aws_access_key_id = **************  
aws_secret_access_key = *******************  
%
% aws configure list  
      Name                    Value             Type    Location  
      ----                    -----             ----    --------  
   profile                             None    None  
access_key     ****************Y47I shared-credentials-file  
secret_key     ****************ti17 shared-credentials-file  
    region                eu-west-2      config-file    ~/.aws/config  


instead of just having default section can have multiple sections (profiles) and use them with  
aws --profile otherone ...  

Examples

list s3 buckets:  
% aws s3 ls  
2020-03-23 15:45:43 bucket1  
2020-03-29 20:10:27 bucket2  
%  
% aws s3 ls s3://bucket1  
2020-04-01 11:23:28         69 index.html  

% aws s3 cp file s3://bucket1/  
% aws s3 rm s3://bucket1/index.html  

% aws s3 sync folder1 s3://bucket1 --exclude *rubbish  
%  

% aws ec2 describe-instances --filters Name=instance-state-name,Values=stopped  --region  eu-west-2  --output json  |  jq  -r  .Reservations[].Instances[] .StateReason.Message% aws ec2 start-instances --instance-ids ...  

% aws iam get-user (lists current users details)  
% aws iam list-users (list all users)  
% aws iam create-user --user-name emusk (create a user)  
% aws iam delete-user --user-name bgates (delete bill)  
% aws ec2 describe-key-pairs (lists key pairs)  
% aws ec2 describe-security-groups (list all sec grps)  
% aws ec2 describe-tags  
%  
