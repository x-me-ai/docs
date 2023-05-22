# **About TwinSync**

Use AI magic to make your existing videos speak any content in any language without training.

TwinSync is an AI-based video editing tool that helps users create custom video content effortlessly.

The core feature of TwinSync is to make any person in your video say anything you want and sync it to any language version using AI technology. Unlike traditional video editing tools, TwinSync requires no lighting, camera or motion capture setup - simply upload your existing videos and start editing.

TwinSync also offers a variety of templates and customization options such as subtitles, text, music, etc. to ensure that your video content is unique. Whether you're a social media manager, brand marketer, or casual user, TwinSync is a powerful creative tool to help you better engage with your audience.

# **API documentation**

This is the API documentation for a video face-shift interface. Upload the cloud addresses of an audio file and a video file, as well as a secret key, to receive the API interface for the synthesized video.

## **POST service request**

**URL address**

http://api.twinsync.xyz:51916/faceswap_video

**Request method**

POST

**Request parameters**

| **Parameter name** | **Type** | **Required or not** | **Description** |
| --- | --- | --- | --- |
| image_url | string | Yes | Cloud address of the photo file |
| video_url | string | Yes | Cloud address of the video file |
| key | string | Yes | String used to verify customer information |

**Response example**

{

"task_id": "20230423_11_23_05_10279",

"result_code": 100,

"msg": "SUCCESS! The video is currently in the queue waiting to be processed."

}

**Error code**

| **Error code** | **Description** |
| --- | --- |
| 200 | Request parameter error or incomplete |
| 201 | Invalid or missing secret key |
| 202 | Invalid or missing cloud address for audio or video file |
| 203 | Internal server error (for example: video conversion failure, storage failure) |

## **GET Status**

**URL address**

http://api.twinsync.xyz:51916/faceswap_video?taskID=***

**Request method**

GET

**Response example**

{

"result_url": "http://api.twinsync.xyz:41748/example/20230423_11_23_05_10279.mp4",

"result_code": 100,

"msg": "taskID:20230423_11_23_05_10279 has been completed, and the output video url is: http://api.twinsync.xyz:41748/example/20230423_11_23_05_10279.mp4"

}

**Status code**

| **Status code** | **Description** |
| --- | --- |
| 101 | The task is in a waiting state |
| 102 | The task is in a running state |
| 103 | The task is in a successful state |
| 104 | The task is in a failed state |
| 200 | The taskID field is missing or the field is not in the service queue |


## **Example Code**
Here's an example Python code snippet showing how to call the TwinSync video face-shift API using the requests library:

import requests

# Set the endpoint URL
url = "http://api.twinsync.xyz:51916/faceswap_video"

# Set the request parameters
params = {
    "image_url": "cloud/photo.jpg",
    "video_url": "cloud/video.mp4",
    "key": "your_secret_key"
}

# Send a POST request to the endpoint with the parameters
response = requests.post(url, data=params)

# Check if the response was successful (status code 100)
if response.status_code == 100:
    # Get the task ID from the response
    task_id = response.json()["task_id"]
    print("Task ID:", task_id)
else:
    print("Error:", response.json()["msg"])

# Wait for a few seconds or minutes before checking the status

# Set the endpoint URL for getting the status
status_url = f"http://api.twinsync.xyz:51916/faceswap_video?taskID={task_id}"

# Send a GET request to the status endpoint
status_response = requests.get(status_url)

# Check if the response was successful (status code 100)
if status_response.status_code == 100:
    # Get the result URL and message from the response
    result_url = status_response.json()["result_url"]
    msg = status_response.json()["msg"]
    print(msg)
    print("Result URL:", result_url)
else:
    print("Error:", status_response.json()["msg"])