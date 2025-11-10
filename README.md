# 🌊 BlueWave-Aquatics — Static Website Hosting on AWS S3

This project hosts a **static website** on **Amazon S3**, providing fast, secure, and cost-efficient global delivery.  
It uses the **AWS CLI** to create, configure, and deploy your static site without the need for Terraform or complex infrastructure tools.

---

## 📁 File Structure

Your project directory should look like this.  
The `bluewave-aquatics` folder contains all of your static content (HTML, CSS, JS, and images):

```
bluewave-aquatics/
├── index.html
├── 404.html
├── css/
│   └── styles.css
├── js/
│   └── main.js
└── images/
    └── logo.png
```

---

## 🧰 Prerequisites

Before deploying your site, make sure you have the following installed and configured:

1. **AWS CLI**  
   👉 [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

2. **AWS Credentials**  
   Configure your AWS credentials locally by running:
   ```bash
   aws configure
   ```
   You’ll need your:
   - AWS Access Key ID  
   - AWS Secret Access Key  
   - Default region (e.g., `us-east-1`)

3. **S3 Bucket Name**  
   Choose a globally unique name, e.g. `bluewave-aquatics-static`.

---

## 🚀 Deployment Steps

Follow these steps to create and host your static website.

### 1️⃣ Create an S3 Bucket
```bash
aws s3 mb s3://bluewave-aquatics-static --region us-east-1
```

### 2️⃣ Enable Static Website Hosting
```bash
aws s3 website s3://bluewave-aquatics-static/ --index-document index.html --error-document 404.html
```

### 3️⃣ Set Bucket Policy for Public Read Access
Create a file named `bucket-policy.json` with the following content (replace `bluewave-aquatics-static` with your bucket name):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::bluewave-aquatics-static/*"
    }
  ]
}
```

Apply the policy:
```bash
aws s3api put-bucket-policy --bucket bluewave-aquatics-static --policy file://bucket-policy.json
```

### 4️⃣ Upload Your Website Files
Use the `sync` command to upload all files from your project folder:
```bash
aws s3 sync ./bluewave-aquatics s3://bluewave-aquatics-static --acl public-read --delete
```

### 5️⃣ Verify Website Endpoint
After deployment, get your public endpoint:
```bash
aws s3 website s3://bluewave-aquatics-static/ --region us-east-1
```

Your site will be available at:
```
http://bluewave-aquatics-static.s3-website-us-east-1.amazonaws.com
```

---

## 🌐 Optional Enhancements

- **Use Amazon CloudFront**: Add a CDN layer for HTTPS, caching, and faster global performance.  
- **Add Route 53**: Point your custom domain (e.g., `www.bluewave-aquatics.com`) to your CloudFront or S3 endpoint.  
- **Enable Logging**: Turn on S3 access logging for insights and monitoring.  
- **Automate with GitHub Actions**: Add a workflow to sync updates to S3 automatically on every commit.

---

## 🧹 Cleanup

To remove your website and avoid charges:
```bash
aws s3 rb s3://bluewave-aquatics-static --force
```

---

## 🧾 License

This project is licensed under the **MIT License**. See the `LICENSE` file for details.

---

🏄‍♂️ *BlueWave-Aquatics — fast, simple, and serverless web hosting on AWS S3.*
