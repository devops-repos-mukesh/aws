# AWS Transcribe – Audio & Video Transcription using Amazon S3

## Overview

Amazon Transcribe is a fully managed Automatic Speech Recognition (ASR) service provided by AWS that converts speech into text. It supports both **batch** and **streaming** transcription and can process audio or video files stored in Amazon S3.

Amazon Transcribe automatically generates transcripts with punctuation, timestamps, and speaker identification (optional). The generated transcript can be used for subtitles, search, analytics, meeting summaries, customer support analysis, and much more.

---

# Features

- Batch transcription
- Real-time (streaming) transcription
- Automatic language identification
- Speaker identification
- Custom vocabulary
- Custom language models
- Content redaction (PII)
- Subtitle generation (SRT & VTT)
- Multi-language support

---

# Supported Input Formats

## Audio

- MP3
- WAV
- FLAC
- AMR
- OGG

## Video

- MP4
- MOV
- AVI
- MKV
- WebM

---

# PoC Overview

This PoC demonstrates:

1. Uploading an **MP3 audio file** to Amazon S3 and generating a transcript.
2. Uploading an **MP4 video file** to Amazon S3 and generating a transcript.
3. Creating transcription jobs using:
   - AWS Management Console
   - AWS CLI

---

# Architecture

<img width="948" height="350" alt="Untitled-2026-07-24-1323" src="https://github.com/user-attachments/assets/698f9f32-7bf7-4dea-82aa-3fc271372dea" />


---

# Prerequisites

- AWS Account
- IAM User with Amazon Transcribe permissions
- Amazon S3 Bucket
- AWS CLI installed (for CLI approach)

---

# Part 1 – Audio Transcription Using AWS Console

## Step 1 – Upload Audio File to Amazon S3

1. Open the AWS Management Console.
2. Navigate to **Amazon S3**.
3. Open your bucket.
4. Click **Upload**.
5. Select your **MP3** file.
6. Upload the file.

Example:

<img width="1920" height="845" alt="image" src="https://github.com/user-attachments/assets/f0bc4196-a594-4d56-92b9-561f4dc84239" />

---

## Step 2 – Open Amazon Transcribe

Navigate to:

```
AWS Console
    └── Amazon Transcribe
```

---

## Step 3 – Create a Transcription Job

Click:

```
Create job
```

Provide the following details:

| Parameter | Value |
|-----------|-------|
| Job Name | audio-transcription-job |
| Language | English (US) |
| Input Type | Amazon S3 |
| Media Format | MP3 |
| S3 URI | s3://my-transcribe-demo/audio/sample-audio.mp3 |

After creation of job it'll look like this - 

<img width="1920" height="845" alt="image" src="https://github.com/user-attachments/assets/fa027f2b-08b4-4859-9a46-3578b6e3fca3" />

---

## Step 4 – Output Configuration

Choose:

- Default output location

or

- Custom S3 bucket for transcript output

---

## Step 5 – Create the Job

Click:

```
Create job
```

The job status will change from:

```
QUEUED
```

↓

```
IN PROGRESS
```

↓

```
COMPLETED
```

---

## Step 6 – View the Transcript

Once the job completes:

- Open the job details.
- View the generated transcript.
- Download the JSON transcript if required.

<img width="1920" height="845" alt="image" src="https://github.com/user-attachments/assets/5a8740d2-949e-4172-aa50-4a7841a5e7e8" />

---

# Part 2 – Video Transcription Using AWS Console

The process is identical to audio transcription.

## Step 1

Upload the **MP4** file to Amazon S3.

Example:

<img width="1920" height="845" alt="image" src="https://github.com/user-attachments/assets/f0bc4196-a594-4d56-92b9-561f4dc84239" />


---

## Step 2

Create a new transcription job.

Example configuration:

| Parameter | Value |
|-----------|-------|
| Job Name | video-transcription-job |
| Media Format | MP4 |
| Language | English |
| Input Location | S3 URI |

<img width="1920" height="845" alt="image" src="https://github.com/user-attachments/assets/7687488b-57ec-471c-8a3b-0a2b82f10fd7" />

<img width="1920" height="845" alt="image" src="https://github.com/user-attachments/assets/39f7260f-5db9-488b-9060-6e10de1fbd16" />

---

## Step 3

Submit the job.

Amazon Transcribe extracts the audio from the video and generates the transcript.

---

## Step 4

Download the transcript from the output bucket or view it in the AWS Console.

<img width="1920" height="845" alt="image" src="https://github.com/user-attachments/assets/3f597bf1-6c97-4f7a-908b-67624fada11e" />

---

# Example Output

<img width="1920" height="845" alt="image" src="https://github.com/user-attachments/assets/a4704cc2-785d-4944-927f-3318fdbd8c22" />

---

# Creating a Transcription Job Using AWS CLI

## Start an Audio Transcription Job

```bash
aws transcribe start-transcription-job \
    --transcription-job-name audio-transcription-job \
    --language-code en-US \
    --media MediaFileUri=s3://my-transcribe-demo/audio/sample-audio.mp3 \
    --output-bucket-name my-transcribe-output
```

---

## Start a Video Transcription Job

```bash
aws transcribe start-transcription-job \
    --transcription-job-name video-transcription-job \
    --language-code en-US \
    --media MediaFileUri=s3://my-transcribe-demo/video/sample-video.mp4 \
    --output-bucket-name my-transcribe-output
```

---

## Check Job Status

```bash
aws transcribe get-transcription-job \
    --transcription-job-name audio-transcription-job
```

Example output:

```json
{
    "TranscriptionJob": {
        "TranscriptionJobName": "audio-transcription-job",
        "TranscriptionJobStatus": "COMPLETED"
    }
}
```

---

## List All Transcription Jobs

```bash
aws transcribe list-transcription-jobs
```

---

## Delete a Transcription Job

```bash
aws transcribe delete-transcription-job \
    --transcription-job-name audio-transcription-job
```

---

# Output Formats

Amazon Transcribe primarily generates transcripts in **JSON** format.

Example:

```json
{
  "results": {
    "transcripts": [
      {
        "transcript": "Hello everyone. Welcome to today's meeting."
      }
    ]
  }
}
```

The transcript can also be converted into:

- TXT
- SRT (Subtitles)
- VTT (Web Captions)

---

# Common Use Cases

- Meeting transcription
- Video subtitles
- Podcast transcription
- Customer support analysis
- Call center analytics
- Voice search
- Compliance recording
- Interview transcription
- Medical dictation
- Educational content transcription

---

# Best Practices

- Store media files in Amazon S3.
- Use descriptive job names.
- Enable speaker labels when multiple speakers are present.
- Use custom vocabularies for domain-specific terms.
- Store transcripts in a separate S3 bucket.
- Delete completed jobs if they are no longer required.

---

# Limitations

- Input media must be accessible from Amazon S3.
- Unsupported media formats cannot be processed.
- Large files take longer to transcribe.
- Streaming transcription requires the Amazon Transcribe Streaming API.

---

# Conclusion

In this PoC, we successfully:

- Uploaded an **MP3 audio file** to Amazon S3 and generated its transcript using Amazon Transcribe.
- Uploaded an **MP4 video file** to Amazon S3 and generated its transcript.
- Learned how to perform the same workflow using both the **AWS Management Console** and the **AWS CLI**.

Amazon Transcribe provides an easy and scalable solution for converting speech from audio and video into accurate text, making it suitable for a wide range of business and analytics applications.
