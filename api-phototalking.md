# **About TwinSync**

Use AI magic to make your existing videos speak any content in any language without training.

TwinSync is an AI-based video editing tool that helps users create custom video content effortlessly.

The core feature of TwinSync is to make any person in your video say anything you want and sync it to any language version using AI technology. Unlike traditional video editing tools, TwinSync requires no lighting, camera or motion capture setup - simply upload your existing videos and start editing.

TwinSync also offers a variety of templates and customization options such as subtitles, text, music, etc. to ensure that your video content is unique. Whether you're a social media manager, brand marketer, or casual user, TwinSync is a powerful creative tool to help you better engage with your audience.

# **API Documentation**

This API document is for the photo talking interface. Upload text, photo cloud address, speaker and key to get the API interface for synthesizing videos.

## **POST Service Request**

**URL Address**

http://api2.sam-sara.cn:17374/phototalking

**Request Method**

POST

**Request Parameters**

| **Parameter Name** | **Type** | **Required (or Mandatory)** | **Description** |
| --- | --- | --- | --- |
| text | string | Yes | Spoken text must not exceed 150 words. |
| image_url | string | Yes | Cloud address of image file |
| pronouncer | string | Yes | Select the voice from Microsoft TTS. |
| key | string | Yes | String for validating client information |

**Response Example**

{

"task_id": "20230423_11_23_05_10279",

"result_code": 100,

"msg": "SUCCESS! The video is currently in the queue waiting to be processed."

}

**Error Code**

| **Error Code** | **Description** |
| --- | --- |
| 200 | Request parameter error or incomplete |
| 201 | Invalid or missing access key |
| 202 | Invalid or missing cloud address of audio or video file |
| 203 | Internal server error (such as video conversion failure, storage failure) |

## **GET Status**

**URL Address**

http://api2.sam-sara.cn:17374/phototalking?taskID=***

**Request Method**

GET

**Response Example**

{

"result_url": "http://api.twinsync.xyz:27323/example/20230423_11_23_05_10279.mp4",

"result_code": 100,

"msg": "taskID:20230423_11_23_05_10279 has been completed, and the output video url is: http://api.twinsync.xyz:27323/example/20230423_11_23_05_10279.mp4"

}

**Status Code**

| **Status Code** | **Description** |
| --- | --- |
| 101 | The task is in a waiting state |
| 102 | The task is in a running state |
| 103 | The task is in a successful state |
| 104 | The task is in a failed state. |
| 200 | taskID field is missing or the field is not in the service queue. |


## **Example Code**

Here's an example python code snippet showing how to call the TwinSync Photo Talking API using the requests library:

    import time
    import requests

    # Set the endpoint URL
    url = "http://api2.sam-sara.cn:17374/phototalking"

    # Set the request parameters
    params = {
        "text": "你好，我是会说话的照片数字人",
        "image_url": "https://twinsync.oss-cn-hangzhou.aliyuncs.com/video/1683730315image.jpeg",
        "pronouncer": "zh-CN-XiaohanNeural",
        "key": "your_access_key"
    }

    # Send a POST request to the endpoint with the parameters
    response = requests.post(url, data=params)
    print('Received response results: ', response.json())

    task_id = None
    # Check if the response was successful (status code 100)
    if response.json()['result_code'] == 100:
        # Get the task ID from the response
        task_id = response.json()["task_id"]
        print("Task ID:", task_id)
    else:
        print("Error:", response.json()["msg"])

    if task_id:
      # Wait for a few seconds or minutes before checking the status
      result_url = None
      while not result_url:

        # Set the endpoint URL for getting the status
        status_url = f"http://api2.sam-sara.cn:17374/phototalking?taskID={task_id}"

        # Send a GET request to the status endpoint
        status_response = requests.get(status_url)
        print('Received processing results: ', status_response.json())

        # Check if the response was successful (status code 100)
        if status_response.json()['result_code'] == 100:
            # Get the result URL and message from the response
            result_url = status_response.json()["result_url"]
            msg = status_response.json()["msg"]
            print(msg)
            print("Result URL:", result_url)
            break
        else:
            print("Status:", status_response.json()["msg"])
            time.sleep(5)




