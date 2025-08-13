# EC2 Static Website Deployment with Apache, Git, and S3

This project demonstrates how to host a static website on an **Amazon EC2 instance** using **Apache HTTP Server** and version control with **GitHub**.  
The project also stores an image in an **Amazon S3 bucket** with a public-read policy for display on the website.

---

## Prerequisites

- An AWS EC2 instance (Amazon Linux 2 recommended)
- Apache HTTP Server installed
- Git installed
- An AWS S3 bucket with public-read access
- A GitHub repository for storing website code

---

## Steps to Deploy

### 1. Update and Install Required Packages
```bash
sudo yum update -y
sudo yum install httpd -y
sudo yum install git -y
```

### 2. Start and Enable Apache
```bash
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd
```

### 3. Create Website Files
Navigate to the web root directory:
```bash
cd /var/www/html
```
Create the `index.html` file:
```bash
sudo nano index.html
```

Paste the following HTML code:
```html
<html>
<head>
    <title>My Static Website</title>
</head>
<body>
    <h1 style="text-align: center;">Welcome to My Static Website!</h1>

    <div style="text-align: center;">
        <img src="https://avinashtale1999.s3.ap-south-1.amazonaws.com/coffee.jpg" 
             alt="Coffee Image"
             style="width:100%; height:auto">
    </div>
</body>
</html>
```

---

### 4. Set Up Git and Push Code to GitHub
```bash
git config --global user.name "AvinashTale"
git config --global user.email "aatale99@gmail.com"
git config --global --add safe.directory /var/www/html

git init
git branch -M main
git remote add origin https://github.com/AvinashTale99/ec2-static-website.git

git add .
git commit -m "Initial commit"
git push -u origin main
```

---

### 5. Store Image in S3 with Public Access
1. Upload your image (`coffee.jpg`) to the S3 bucket (`avinashtale1999`).
2. Apply the following bucket policy:
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::avinashtale1999/*"
        }
    ]
}
```
3. Make sure the object URL is accessible (e.g.,  
`https://avinashtale1999.s3.ap-south-1.amazonaws.com/coffee.jpg`).

---

## Access Your Website
After completing the steps, open your EC2 public IP in the browser:
```
http://<EC2_PUBLIC_IP>/
```

---

## Project Structure
```
/var/www/html/
├── index.html
```

---

## Notes
- Ensure your EC2 instance’s **Security Group** allows inbound traffic on port **80 (HTTP)**.
- S3 bucket should be in the **ap-south-1** region to match the image URL.
- Apache must be running for the site to be accessible.

---

## License
This project is open-source and free to use.


---

## Author
**Avinash Tale**  
GitHub: [AvinashTale99](https://github.com/AvinashTale99)  
Email: aatale99@gmail.com
