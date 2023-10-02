---
title: "Second"
date: 2023-10-02T19:34:49+01:00
draft: false
---

 How to redirect an apex domain in AWS
- June 21, 2021


Overview

An apex domain is the top (“root”) level of a domain, essentially the base domain that itself does not contain any subdomains. In accredible we have multiple domains - the one we’ll use as an example here is credential.net

https://stuartabramshumphries.com  did not point to a website and so users going to this URL would get a ‘this site could not be reached message’.

there could be multiple websites hosted from this domain - e.g. a.stuartabramshumphries.com, b.stuartabramshumphries.com etc- what we would like to do is redirect  stuartabramshumphries.com to https://www.stuartabramshumphries.com - this means if a customer inadvertently goes to the former they will be redirected to the latter.
Implementation

Normally to redirect a hostname we would use a CNAME or an ALIAS - however we cannot (due to DNS standards) do this with an apex domainname.

What the fix for this is at a high level:

Request → Route 53 → CloudFront → S3 → Redirect → CloudFront → User

So:

(1) user enters the URL in their web browser (e.g. stuartabramshumphries.com )

(2) request goes to the AWS DNS service route53

(3) Route53 points this at an AWS cloudfront distribution

(4) the cloudfront distribution points at an underlying S3 bucket that acts as a website

(5) this s3 bucket then redirects at the correct cloudfront distribution for the www.stuartabramshumphries.com website which is what we want

(6) user now views the correct website
step 1

create an s3 bucket with the name http://stuartabramshumphries.com

set as a static website host

ensure public access is blocked for security (even though it should be empty!)

set redirect of this s3 bucket to www.stuartabramshumphries.com

step 2

create cloudfront distribution:

Note we also create an SSL certificate for this site (if one doesnt exist already - so that we can use it with https)
step 3

we then need to set the A recored for stuartabramshumphries.com to point at this new cloudfront distribution:





stuartabramshumphries.com  A  Simple


 - blah.cloudfront.net





SUMMARY


This will now be working - simply point at https://stuartabramshumphries.com and you’ll automatically be taken to https://www.stuartabramshumphries.com

