☁️ AWS Static Website Hosting — S3 + CloudFront

Show Image
Show Image
Show Image


🌐 Live URL: https://d3co6jb40mmso7.cloudfront.net




📌 Project Overview

This project demonstrates how to host a secure, fast, and globally available static website on AWS using Amazon S3 and Amazon CloudFront.

The portfolio website is served over HTTPS with CloudFront acting as a CDN (Content Delivery Network), while the S3 bucket remains completely private — accessible only through CloudFront using Origin Access Control (OAC).


🏗️ Architecture

User Browser
     │
     ▼
Amazon CloudFront  ◄──── HTTPS + CDN + Global Edge Locations
     │
     │  (Origin Access Control - OAC)
     ▼
Amazon S3 Bucket   ◄──── Private Bucket (No public access)
  index.html


⚙️ AWS Services Used

ServicePurposeAmazon S3Stores the website files (HTML, CSS) in a private bucketAmazon CloudFrontDelivers content globally via edge locations with HTTPSOrigin Access Control (OAC)Restricts S3 access — only CloudFront can read the filesS3 Bucket PolicyIAM policy that enforces OAC securityAWS ACMProvides free SSL/TLS certificate for HTTPS


🔐 Security Design


✅ S3 bucket has Block All Public Access enabled
✅ No S3 static website hosting — bucket is purely private storage
✅ OAC (Origin Access Control) ensures only CloudFront can fetch files
✅ All traffic served over HTTPS — HTTP redirects to HTTPS automatically
✅ Bucket policy uses least privilege — only s3:GetObject allowed



🚀 Step-by-Step Deployment

Step 1 — Create S3 Bucket

- Bucket name: shakeel-portfolio-2024
- Region: ap-south-1 (Mumbai)
- Block ALL public access: Enabled
- Versioning: Disabled

Step 2 — Upload Website Files

- Upload index.html to the bucket root

Step 3 — Create CloudFront Distribution

- Origin: S3 bucket
- Origin Access: OAC (Origin Access Control)
- Viewer Protocol Policy: Redirect HTTP to HTTPS
- Default Root Object: index.html
- WAF: Disabled (cost saving for dev)

Step 4 — Update S3 Bucket Policy

json{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowCloudFrontAccess",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::shakeel-portfolio-2024/*",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceArn": "arn:aws:cloudfront::535337619381:distribution/EMD9RINSGXZ73"
                }
            }
        }
    ]
}

Step 5 — Test

- Open https://d3co6jb40mmso7.cloudfront.net
- Verify HTTPS padlock in browser
- Check x-cache: Hit from cloudfront in response headers


💡 Key Concepts Demonstrated

What is OAC?


Origin Access Control is a CloudFront feature that signs every request to S3. S3 bucket policy only allows requests that are signed by your specific CloudFront distribution — blocking all direct access attempts.



Why CloudFront instead of S3 direct hosting?


S3 direct hosting requires public bucket access (security risk), has no HTTPS on custom domains, and no global caching. CloudFront solves all three problems.



What happens when I update my website?


Upload new file to S3, then create a CloudFront invalidation for /* to clear the cache so users see the latest version immediately.




📊 Cost Breakdown

ServiceFree TierCost After Free TierS3 Storage5 GB free~$0.023/GB/monthS3 Requests20,000 GET free~$0.0004/1000 requestsCloudFront1 TB transfer free~$0.0085/GBTotal (this project)~$0/monthVery low


🧠 What I Learned


How to architect a secure static website on AWS
Difference between S3 public hosting vs CloudFront with OAC
How CloudFront edge caching works and how to invalidate cache
Writing least-privilege S3 bucket policies
Importance of HTTPS and how CloudFront + ACM enables it for free



🗂️ Project Structure

shakeel-portfolio/
│
├── index.html          # Main portfolio webpage
└── README.md           # This file


👨‍💻 Author

Mohamed Shakeel
Cloud Engineer | AWS Enthusiast | HCLTech
