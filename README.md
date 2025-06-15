# 📡 CDN Caching Demonstration with HTTP Headers

## Objective

Host content on AWS S3, connect them to CDN77, apply different HTTP headers with Cyberduck, and use Postman/Terminal to see how those headers affect caching behavior.

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
  <img width="950" alt="Amazon S3" src="https://github.com/user-attachments/assets/2c6a176f-e070-4ffd-aea9-9378788d001c" />
  <img width="953" alt="Pasted Graphic 1" src="https://github.com/user-attachments/assets/b82b4690-8687-4983-8289-26c9727c86ec" />


- Enable **Static Website Hosting** and **Public Access**.
  <img width="825" alt="Pasted Graphic 2" src="https://github.com/user-attachments/assets/af974bf9-aea7-4339-80b8-13f93c08c4d2" />

- Use **Cyberduck** (configured with AWS Access Keys) to upload the files:
  - `index.html`
  - `Ironman.jpg`
  - `Thanos.jpg`
  - `stewie.jpg`
<img width="730" alt="Pasted Graphic 3" src="https://github.com/user-attachments/assets/02ca8dbe-4919-4382-a26b-139ee4b43b2b" />
<img width="723" alt="Pasted Graphic 4" src="https://github.com/user-attachments/assets/3cf4db99-8a0d-4e32-a1de-e177ed5c3792" />
<img width="958" alt="Pasted Graphic 5" src="https://github.com/user-attachments/assets/d9b87c87-3a31-4658-8b86-b6774e87806a" />

- Apply the correct **bucket policy** to make all content publicly accessible.
<img width="643" alt="Block public access (bucket settings)" src="https://github.com/user-attachments/assets/1e967590-c800-422b-91e0-e2cc586f14d3" />

- Test the webpage with a basic index.html page using the bucket URL: http://another-webpage-bucket.s3-website.eu-north-1.amazonaws.com
<img width="1106" alt="How A CDN Works" src="https://github.com/user-attachments/assets/18433ac9-fe01-474a-b1eb-920726ee1552" />


### 📦 Apply Initial HTTP Headers

Using **Cyberduck**, set the following headers:

- `Ironman.jpg` → `Cache-Control: max-age=60`
- `Thanos.jpg` → `Cache-Control: no-cache`
<img width="661" alt="Pasted Graphic 8" src="https://github.com/user-attachments/assets/ca3902c3-aa19-4784-bbaa-234e46f7e74b" />
<img width="649" alt="Headers" src="https://github.com/user-attachments/assets/5a254621-4a0d-4049-9a79-31795df2a9e4" />

---

### 🔗 Step 2: Connect S3 Bucket to CDN77

- Use the S3 bucket’s public URL as the **origin** to create a CDN resource in CDN77.
- 
<img width="630" alt="Pasted Graphic 10" src="https://github.com/user-attachments/assets/930aeb81-4021-4488-958d-c94d9c36e5d5" />

- Wait for propagation (~15 minutes).
  
<img width="578" alt="CDN Resource details" src="https://github.com/user-attachments/assets/a42fb0ec-4197-4cb0-9b91-2f2997e5674d" />

- Access the CDN version of the site/images using: https://1107115457.rsc.cdn77.org/
  
<img width="1124" alt="How A CDN Works" src="https://github.com/user-attachments/assets/7ba1adf3-7d41-43e0-abfa-ec4a63f0e96b" />



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
```
Using Postman:

<img width="850" alt="Pasted Graphic 12" src="https://github.com/user-attachments/assets/e5ceeb3f-1fc3-4659-9f1d-12072b932141" />

<img width="842" alt="Pasted Graphic 13" src="https://github.com/user-attachments/assets/b903fdf4-6483-49b7-b341-88fc559c7b2e" />


## 📌 Observing HTTP Headers and Caching Behavior

### 🔍 Key Headers to Monitor

- **`Cache-Control`** – Defines how and if content is cached.
- **`X-77-Cache`** – Indicates whether the request was served from the CDN cache (`HIT`) or directly from the origin (`MISS`).
- **`X-77-Age`** – Shows how long (in seconds) the content has been cached by the CDN.

---

### 🧠 Behavior Analysis

#### 📷 `Thanos.jpg`

- **Header Applied**: `Cache-Control: no-cache`
- **Observed Behavior**:
  - `X-77-Cache: MISS` → The CDN does **not** cache the image.
  - `X-77-Age` is **absent**, confirming that the image is fetched from the **origin (S3)** every time.
- **Conclusion**: The `no-cache` directive prevents CDN-level caching, ensuring that every request fetches the latest version directly from the origin (S3 bucket).

#### 📷 `Ironman.jpg`

- **Header Applied**: `Cache-Control: max-age=60`
- **Observed Behavior**:
  - `X-77-Cache: HIT` → The image **is cached** at the CDN level.
  - `X-77-Age` → Shows how long the image has been stored in the cache (e.g., 38 seconds).
- **Conclusion**: The `max-age=60` header allows the CDN to cache the image for 60 seconds before revalidating it.

---

## 🏷️ Understanding the ETag Header

The **`ETag`** (Entity Tag) is used to determine whether the content has changed. It helps reduce bandwidth by allowing clients (and CDNs) to check if cached content is still valid.

### 🔁 How to Test ETag Behavior

1. Make a small change to `index.html` (e.g., modify some text).
2. Re-upload the updated file to your S3 bucket using **Cyberduck**. The webpage shows the index.html has been changed and updated correctly.

<img width="1067" alt="How A CDN Works" src="https://github.com/user-attachments/assets/a2f75906-a30a-4b88-a6b5-1f6c6e6a1db1" />

3. Purge the file from the CDN cache.
4. Use **Postman** or **curl** to inspect the new `ETag`.

<img width="855" alt="Screenshot 2025-06-14 at 23 27 24" src="https://github.com/user-attachments/assets/5727d4f3-62ea-4205-885e-264504783e99" />

```diff
Old ETag: W/"4ca0a63468d01790e95c9e67356d4218"
New ETag: W/"c6c631917dd001e411bed7e9532ae2db"
```

The change in the ETag confirms that the content was updated and the new version is now being served.

##Additional Notes
-AWS S3 allows you to set or update HTTP headers:
Directly in the AWS Console
Using the AWS CLI

-CDN77 (as well as CloudFront and Cloudflare) allows you to configure HTTP headers at the CDN resource level.
-Using AWS CLI, you can make rapid changes to HTTP headers of multiple files with just one command.
🧰 Example: Update Cache-Control Headers for All Files in a Bucket Using AWS CLI
```diff
aws s3 cp s3://another-webpage-bucket/ s3://another-webpage-bucket/ \
  --recursive \
  --metadata-directive REPLACE \
  --cache-control "max-age=360000"
```

This command recursively updates all files in the bucket with a new Cache-Control value of max-age=360000 (i.e., 100 hours).


