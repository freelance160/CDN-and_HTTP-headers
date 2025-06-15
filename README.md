# 📡 CDN Caching Demo with HTTP Headers

## Objective

Demonstrate how HTTP headers (like `Cache-Control`, `ETag`, etc.) affect caching behavior through a CDN.

---

## 🛠️ Tools & Applications Used

- **AWS S3**
- **Cyberduck**
- **Postman**
- **Terminal (Linux/macOS)**
- **CDN77**

---

## 📁 Content Hosted

- `index.html` – a simple webpage
- `Ironman.jpg`
- `Thanos.jpg`
- `stewie.jpg`

---

## 🧾 Step-by-Step Process

### ✅ Step 1: Create & Upload to S3 Bucket

- Create a new S3 bucket on AWS.
- Enable **Static Website Hosting** and **Public Access**.
- Use **Cyberduck** (configured with AWS Access Keys) to upload the files:
  - `index.html`
  - `Ironman.jpg`
  - `Thanos.jpg`
  - `stewie.jpg`
- Apply the correct **bucket policy** to make all content publicly accessible.
- Test the webpage using the bucket URL: http://another-webpage-bucket.s3-website.eu-north-1.amazonaws.com


### 📦 Apply Initial HTTP Headers

Using **Cyberduck**, set the following headers:

- `Ironman.jpg` → `Cache-Control: max-age=60`
- `Thanos.jpg` → `Cache-Control: no-cache`

---

### 🔗 Step 2: Connect S3 to CDN77

- Use the S3 bucket’s public URL as the **origin** to create a CDN resource in CDN77.
- Wait for propagation (~15 minutes).
- Access the CDN version of the site/images using: https://1107115457.rsc.cdn77.org/


---

### 🧪 Step 3: Test & Inspect with Postman/Terminal

Use **Postman** or **Terminal** to observe HTTP headers behavior without browser caching interference.

#### Example CDN URLs:

- https://1107115457.rsc.cdn77.org/Thanos.jpg
- https://1107115457.rsc.cdn77.org/Ironman.jpg

#### Using Terminal:

```bash
curl -I https://1107115457.rsc.cdn77.org/Thanos.jpg

HTTP/2 200  
date: Sat, 14 Jun 2025 18:58:10 GMT  
content-type: image/jpeg  
content-length: 65764  
x-amz-id-2: XgEnvNdEm6EfLho9Ju0ezW3ZxNi5NbGHs7yjYqUEhH87mZdr82gE3QnGeAdRGhY73Xp8vB+h43s=  
x-amz-request-id: 7EGNTQQJR12YJFPX  
last-modified: Sat, 14 Jun 2025 16:17:26 GMT  
etag: "0445b1470a73433d3ee7d76b2f0fd82b"  
x-amz-server-side-encryption: AES256  
cache-control: no-cache  
x-77-cache: MISS  
x-77-pop: newyorkUSNY  

curl -I https://1107115457.rsc.cdn77.org/Ironman.jpg

HTTP/2 200  
date: Sat, 14 Jun 2025 18:58:28 GMT  
content-type: image/jpeg  
content-length: 77147  
x-amz-id-2: Sz2FDG1EYmU6EveplIdepXADElHWHHQtGbeJpLalajeDzM0VnsH7LsFP8OUG2cdblUBHd9LAQB+vLLbANENY8rtTwcg2bcoz  
x-amz-request-id: 487ZC6W0FF65XZCH  
last-modified: Sat, 14 Jun 2025 16:11:36 GMT  
etag: "506bc40b19934f7cc721d6463c752af9"  
x-amz-server-side-encryption: AES256  
cache-control: public,max-age=60  
x-77-cache: HIT  
x-77-age: 38  
x-77-pop: newyorkUSNY  


