# lambda-tutorial-app
lambda tutorial Application
building a user registration API with API Gateway, lambda and DynamoDB
+------------------------ AWS Architecture -------------------------+
|                                                                   |
|  [User]                                                           |
|     |                                                             |
|     v                                                             |
| +----------+      +-----------+      +------------+               |
| | Route 53 | ---> | CloudFront| ---> | API Gateway|               |
| +----------+      +-----------+      +------------+               |
|       |                 |                  |                      |
|       v                 v                  v                      |
|    [S3 Bucket]   [Certificate Mgr]     +--------+                 |
|                                        | Lambda | ----+           |
|                                        +--------+     |           |
|                                             ^         v           |
|                                             |     +--------+      |
|                                             +---->|DynamoDB|      |
|                                                   +--------+      |
|                                                                   |
+-------------------------------------------------------------------+
HOW I SOLVE THE PROBLEM
- WHEN AND CREAT DYNAMODB
- when and create role lambda link to cloudwatchfull, amazondynamodbfull
- go to lambda an create,sellect the role u created,past the two  python code on the diffn file.  remember ur lambda name
- go to ur api gateway creat api, creat resource (always stand on / bf creat e resources),creat methode
- enable cors, (stand on each method as u enable cors)
- create s3 host the content, give permission, do the property
  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::bucket-name/*"      
        }
    ]

}
NB  PLEASE ON THE BUCKET-NAME IN LINE 38 REPLACE WITH EITHER UR BUCKET, CLOUDFRONT URL, ALB URL , TO BE ABLE TO VIEW ON BROWSER
NB IF U WILL USE CLOUDFRONT AS UR ENTRYPOINT, THEN MAKE SURE U BLOCK UR PUBLIC ACCESS OF UR S3

- CREATE UR CLOUDFRONT
      0 Enter your distribution name and choose single website or app for distributions options then click on next
      0 For Origin type choose Amazon S3,  Under the Origin settings section, click on the Origin Domain field and select the name of the S3 bucket you created earlier.
    0 Leave the Origin Path field blank. ,On Origin access, choose Origin access control settings. Then, click on Create new OAC
    0 Then, click on create in the dialog box that opened 
For the field Enable Origin shield, select NO
Scroll down to the Default Cache Behavior settings section. For Viewer Protocol Policy, select Redirect HTTP to HTTPS.
    0 In the Web Application Firewall section, choose Do not enable security protections just for the demo.
    0 Next, scroll down to Settings. Inside the Default Root Object field, enter the filename at the root level, which should be your landing page. I used index.html as my Default Root Object.
   0 Leave the rest of the options as default and click on Create Distribution.

    0 Note: Once the distribution is created, it will show the policy to set on the S3 bucket to give access to the CloudFront Distribution
Copy the policy and let’s set it in the S3 bucket

0 Go back to S3 service and select your S3 bucket name you created earlier
Click on the Permissions tab then, scroll down to Bucket policy and click on Edit
Paste the copied policy and then click on Save changes


              Part 2: Serve the website using a custom domain name with Route 53
                 0 a) Create a Hosted zone in Route 53
                   In the Domain name field, enter the domain name you registered on Namecheap
Set Type as Public and click on Create hosted zone

              b) Configure the nameservers in your domain name provider
To do that, follow these steps:
On the Hosted zones dashboard click on the hosted zone you created 
On the dashboard, you will see the records created automatically by AWS when you created the hosted zone
Select the record with the type NS. On the sidebar Record details, you will see the NS values you will add to the Namecheap name servers.

Now, go to Namecheap dashboard domain list, then click on the MANAGE button of your domain name


Now, scroll down to the NAMESERVERS field and click on the arrow down
Select Custom DNS.

Now copy and paste the nameservers from AWS one after the other (Without the . at the end), then click on the tick to save

      ## CONFIGURE UR DOMAIN NAME IN CLOUDFRONT
3- Add an alternate domain name on CloudFront
Here, we are going to add the new domain name created on Namecheap as an alternate domain name on CloudFront. So that we can access our website from that domain name as well.
Go to the CloudFront management dashboard
Click on the distribution you created
In the Settings section under General tab, click on Edit
On the Alternate domain field, click on Add item then fill in the domain name you registered on Namecheap

    # REQUESTING CERTIFICATE
 0Scroll down to Custom SSL and click on Request certificate
 0You will be taken to a new tab; select Request a public certificate, then click on Next
 0In the Domain names section, enter the domain name in the Fully qualified domain name field, then scroll down and click on Request to create the certificate.
 0 You can click on View certificate to see the certificate details. The certificate shows Pending at the beginning, and once it is issued, the status changes to issued.
 0 In the certificate you just created, scroll down to the Domains section and click on Create records in Route 53.
 0Then click on the Create records button
 0 Return to the previous tab from where you were redirected (Settings for the distribution), select the certificate you just created for the custom SSL, and click on Save changes.
0 After saving the changes, the distribution will be updated, and the modifications will be deployed


## Setting records on Route 53 to direct traffic to CloudFront
  0  In your domain-hosted zones in Route 53, click on Create Record.
  0 We will be creating it as an A record for IPv4, and we’ll select the Alias option.
 In the Alias Target (Route traffic to), select Alias to CloudFront distribution
Then, select your CloudFront distribution and click on Create records
GO TO THE BROWSER WITH UR DOMAIN

https://github.com/utrains/schoolstatic.GIT
