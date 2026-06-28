# Portfolio Website — Hosted on AWS

Live site → https://d3co6jb40mmso7.cloudfront.net

---

What is this project?

This is my personal portfolio website hosted entirely on AWS. I built this to get hands-on experience with cloud hosting instead of just reading about it. The site runs on S3 and CloudFront — no traditional web server, no EC2, no monthly server bill.

---
 How it works

The HTML file sits in a private S3 bucket. Nobody can access that bucket directly from the internet. CloudFront sits in front of it and serves the website to users over HTTPS. That's it.

```
You (browser)  →  CloudFront  →  S3 bucket (private)
```

The reason I kept S3 private is simple — there's no reason for anyone to hit S3 directly. CloudFront handles everything: caching, HTTPS, and fast delivery from edge locations closer to the user.

---
 What I used

- **S3** — stores the index.html file
- **CloudFront** — serves it to the world over HTTPS
- **Origin Access Control (OAC)** — makes sure only CloudFront can read from S3
- **S3 Bucket Policy** — enforces the OAC rule at the bucket level

---
 Why CloudFront instead of just making S3 public?

A few reasons:

1. Public S3 buckets are a bad habit. Most data breaches involving S3 happen because someone made a bucket public that shouldn't be.
2. CloudFront gives you HTTPS for free. S3 static hosting doesn't support HTTPS on its own.
3. CloudFront caches your content at edge locations — so someone opening the site from Europe doesn't have to wait for a response from Mumbai.

---

 Setting it up
Nothing fancy here. I created the bucket in Mumbai region, uploaded the HTML file, then created a CloudFront distribution pointing to it with OAC enabled. AWS generates the bucket policy for you — you just paste it in.

One thing that caught me off guard: after creating the distribution, I got an AccessDenied error. Turned out the bucket policy wasn't applied automatically even though the console said it would be. Fixed it by adding the policy manually. Worth knowing if you're building this yourself.

---

 Cost

Basically zero. S3 and CloudFront both have generous free tiers. For a portfolio site with low traffic, you're looking at a few cents a month at most — probably nothing.

---



## About me

I'm Mohamed Shakeel, a cloud engineer based in India. Now I'm building real hands-on experience rather than just collecting certifications.

