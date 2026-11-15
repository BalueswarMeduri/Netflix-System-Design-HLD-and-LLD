# Netflix System Design – HLD and LLD

## Functional Requirements
- Users should be able to upload videos.
- Users should be able to watch videos.
- The system should support adaptive bitrate streaming.

## Non-Functional Requirements
- **Scalability:** The system should handle millions of users and video uploads.
- **Availability and Reliability:** Users should be able to access and watch videos without frequent interruptions.
- **Low Latency:** Videos should start quickly, with minimal buffering.

## 1. Where Should We Store the Videos?

We will use **Amazon S3 (Object Storage)** to store video files.

We don't store the actual video files in a traditional relational database like MySQL or PostgreSQL because video files are large and require a storage system designed for such objects.

Instead:
- **Amazon S3:** Stores the actual video files.
- **Database:** Stores video metadata, such as video title, description, upload status, S3 object key, and other details.

## 2. Who Should Upload the Videos?

We have two possible approaches:

1. Client-side upload
2. Server-side upload

**Option 1: Client-Side Upload**

In this approach, the client uploads the video directly to Amazon S3.

The flow is:

1. The client requests permission to upload a video from the backend.
2. The backend generates a pre-signed S3 URL.
3. The backend sends the pre-signed URL to the client.
4. The client uses that URL to upload the video directly to S3.

**Option 2: Server-Side Upload**

In this approach, the client first sends the video to our backend server.

The backend receives the video and then uploads it to Amazon S3.

The flow is:

1. The client sends the video to the backend.
2. The backend receives and processes the video.
3. The backend uploads the video to Amazon S3.

This approach requires the video data to pass through our backend, which increases server bandwidth usage and processing overhead.

**Which approach should we choose?**

We will choose **Client-Side Upload using Pre-Signed URLs**.

Why?

- The video is uploaded directly to S3.
- Our backend doesn't need to handle large video files.
- It reduces backend bandwidth and processing overhead.
- It makes the system easier to scale.

This is a better approach for a video streaming platform like Netflix.
