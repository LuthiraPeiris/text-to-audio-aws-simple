# 📄 Text-to-Audio Convertor Web App using AWS (PDF to MP3 + Email Delivery)

<p align="center">
  <strong>A serverless AWS application that converts PDF and DOCX documents into MP3 audio using Amazon Polly and delivers the generated audio through email.</strong>
</p>

---

## Architecture
![Image Alt](https://github.com/LuthiraPeiris/text-to-audio-aws-simple/blob/23d390016dc7ddb7f7b7a379983a7451648cfa4b/image/diagram.png)


<p align="center">
  <img src="https://img.shields.io/badge/AWS-Serverless-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS"/>
  <img src="https://img.shields.io/badge/Amazon_S3-Storage-569A31?style=for-the-badge&logo=amazons3&logoColor=white" alt="Amazon S3"/>
  <img src="https://img.shields.io/badge/AWS_Lambda-Python-FF9900?style=for-the-badge&logo=awslambda&logoColor=white" alt="AWS Lambda"/>
  <img src="https://img.shields.io/badge/Amazon_Polly-TTS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" alt="Amazon Polly"/>
  <img src="https://img.shields.io/badge/Amazon_SES-Email-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" alt="Amazon SES"/>
  <img src="https://img.shields.io/badge/API_Gateway-API-FF4F8B?style=for-the-badge&logo=amazonaws&logoColor=white" alt="API Gateway"/>
</p>

---

This project is a **real-world, serverless web application** built on AWS that allows users to:

* Upload a **PDF or DOCX** file
* Select a **Polly voice**
* Convert the document's text into **MP3 audio**
* Automatically **email the generated audio file** to the user

---

## 📌 Project Overview

The **Text-to-Audio Converter** is a serverless web application built using AWS services.

Users can upload a PDF or DOCX document, select an Amazon Polly voice, and provide an email address. The application extracts the document's text, converts it into speech using Amazon Polly, stores the generated MP3 file in Amazon S3, and sends the user an email containing a temporary download link.

The application demonstrates how multiple managed AWS services can be combined to create an event-driven, serverless workflow without managing traditional application servers.

### Core Workflow

```text
Document Upload
      ↓
Amazon S3
      ↓
S3 Event Notification
      ↓
AWS Lambda
      ↓
Text Extraction
      ↓
Amazon Polly
      ↓
MP3 Generation
      ↓
Amazon S3
      ↓
Presigned Download URL
      ↓
Amazon SES
      ↓
User Email

---

## 🧰 Tech Stack

| Layer      | Technology            |
| ---------- | --------------------- |
| Frontend   | HTML, CSS, JavaScript |
| Backend    | AWS Lambda (Python)   |
| Storage    | Amazon S3             |
| TTS Engine | Amazon Polly          |
| Email      | Amazon SES            |
| API        | Amazon API Gateway    |

---

## 🧐 How It Works

1. **User uploads a document** via the web interface and selects a voice + email.
2. **Frontend calls an API Gateway endpoint**, which invokes a Lambda function to generate a **presigned S3 upload URL**.
3. The document is **uploaded to S3** using the presigned URL.
4. S3 triggers another Lambda function (`convertTextToAudio`) when a file is uploaded to `uploads/`.
5. This function:

   * Extracts text from the file (using PyPDF2 for PDF or docx2txt for DOCX)
   * Sends the text to **Amazon Polly** to generate audio
   * Uploads the MP3 to `audio/` in S3
   * Generates a presigned download link for the audio
   * Sends an email via **Amazon SES** with the link

---

## 💪 Features

* ✅ PDF and DOCX file support
* ✅ Upload via secure, presigned S3 URLs
* ✅ Choose from multiple Amazon Polly voices
* ✅ Email delivery with clickable download/play link
* ✅ No servers to manage (fully serverless)

---

## 🔐 Security

* ✅ S3 Bucket is private with **Block All Public Access** enabled
* ✅ All access is granted via **temporary presigned URLs**
* ✅ IAM policies restrict each Lambda to only necessary actions

---

## 🧲 Environment Variables

These should be set in the Lambda configuration:

| Variable       | Description                              |
| -------------- | ---------------------------------------- |
| `BUCKET_NAME`  | Name of your S3 bucket                   |
| `SENDER_EMAIL` | Verified SES email to send messages from |

---

## 📨 Sample Email Output

* Subject: `Your Audio is Ready!`
* Body: Styled HTML message with download/play button
* Link is valid for 1 hour (presigned URL)

---

## 🚀 Deployment Notes

* Deploy Lambda functions via AWS Console or SAM/CDK
* Configure S3 bucket with appropriate **CORS** and **Event triggers**
* Verify sender & recipient emails in **Amazon SES** (if in sandbox mode)

---
