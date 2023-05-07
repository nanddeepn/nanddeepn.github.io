---
title: "Generate Images in SharePoint with OpenAI API DALL-E and Power Automate"
date: "2023-04-23"
share: true
header:
  image: media/2023-04-23-test-images-spo-openai-dalle-power-automate/overview.png
  teaser: media/2023-04-23-test-images-spo-openai-dalle-power-automate/overview.png
categories:
  - SharePoint
  - Power Automate
  - OpenAI
tags:
  - "2023"
  - April 2023
last_modified_at: 2023-04-23T00:00:00-00:00
---
## Introduction

In SharePoint, images play an important role in standing out your content. We often need sample images to support the news and articles. OpenAI API DALL-E is the best fit in this case to generate images related to any topic.

In this article, we will explore how OpenAI API DALL-E can be combined with Power Automate to generate images in SharePoint.


## Get the API Key

Follow the below steps to get the API key to work with Open AI APIs.

1. Navigate to [OpenAI](https://platform.openai.com/overview). Sign up, or register to proceed.

2. From your profile, select **View API Keys**.

    ![](/media/2023-02-12-test-docs-spo-openai-power-Automate/01.png)

3. Click **+ Create new secret key** to create one. Note down the secret key for future use.

    ![](/media/2023-02-12-test-docs-spo-openai-power-Automate/02.png)


## OpenAI Images - API Reference

OpenAI provides valuable [documentation](https://platform.openai.com/docs/api-reference/images) on generating images.

**Create image**

An image can be generate by using below API:

```
POST https://api.openai.com/v1/images/generations
```

Pass the minimum request body as follows:

```json
{
  "prompt": "<prompt>",
  "n": [n],
  "size": [size]
}
```

The request body parameters can be explained as follows:

- `prompt` (required): string representing description of the image to generate. (Maximum 1000 characters)
- `n` (optional): Number of images to generate between `1` to `10`. Defaults `1`.
- `size` (optional): Size of the image to generate. Allowed values: `256x256`, `512x512`, or `1024x1024`. Defaults `1024x1024`.

Below sample request will generate 3 images of duck swimming in the pond.

```json
{
  "prompt": "duck swimming in the pond",
  "n": 3,
  "size": "1024x1024"
}
```

We will use these APIs to generate the image and upload it to SharePoint using Power Automate.

## Power Automate

We will now develop a Power Automate flow to make a request to OpenAI API DALL-E to generate the images, which then can be uploaded to SharePoint.

Follow the below steps to set up the Power Automate flow:

1. We will start by creating an instant cloud flow and add manual inputs as follows:
    - `prompt`: description of the image(s) to generate.
    - `Number of files`: Number of images to generate.
    - `File Name`: Format to follow for the file name. (e.g. input of `duck-swim` will generate images as `duck-swim-1`, `duck-swim-2`, and so on based on value specified for the `Number of files` parameter).

    ![](/media/2023-04-23-test-images-spo-openai-dalle-power-automate/01.png)

2. In the next step, we will use `HTTP` action to post the request to OpenAI API. This will help us to get the image URLs from OpenAI.

    ![](/media/2023-04-23-test-images-spo-openai-dalle-power-automate/02.png)

    The sample response output is generated as follows:

    ```json
    {
        "created": 1682092329,
        "data": [
            {
            "url": "https://oaidalleapiprodscus.blob.core.windows.net/private/org-ySsyovfx0gPXFuCeIjTkZHc5/user-iPdZMyLdUIzWjPqpFTVCmfKN/img-40Jnt8gQ4bZc1LwJPpCXqSjY.png?st=2023-04-21T14%3A52%3A09Z&se=2023-04-21T16%3A52%3A09Z&sp=r&sv=2021-08-06&sr=b&rscd=inline&rsct=image/png&skoid=6aaadede-4fb3-4698-a8f6-684d7786b067&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2023-04-21T11%3A32%3A50Z&ske=2023-04-22T11%3A32%3A50Z&sks=b&skv=2021-08-06&sig=Tsp6GR3CN50wHlR2OeXZ7gK3FIII57ZCxvxwn2uzzVk%3D"
            },
            {
            "url": "https://oaidalleapiprodscus.blob.core.windows.net/private/org-ySsyovfx0gPXFuCeIjTkZHc5/user-iPdZMyLdUIzWjPqpFTVCmfKN/img-hfbm3nB1VHhyeL6feYSJ58u6.png?st=2023-04-21T14%3A52%3A09Z&se=2023-04-21T16%3A52%3A09Z&sp=r&sv=2021-08-06&sr=b&rscd=inline&rsct=image/png&skoid=6aaadede-4fb3-4698-a8f6-684d7786b067&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2023-04-21T11%3A32%3A50Z&ske=2023-04-22T11%3A32%3A50Z&sks=b&skv=2021-08-06&sig=xPzb4TDMQfeNXT1bcDsbLrLbgMNEwmPfppL8RBgGRM8%3D"
            },
            {
            "url": "https://oaidalleapiprodscus.blob.core.windows.net/private/org-ySsyovfx0gPXFuCeIjTkZHc5/user-iPdZMyLdUIzWjPqpFTVCmfKN/img-EtN6QcSBTCJ0d0UHjreoLXVg.png?st=2023-04-21T14%3A52%3A09Z&se=2023-04-21T16%3A52%3A09Z&sp=r&sv=2021-08-06&sr=b&rscd=inline&rsct=image/png&skoid=6aaadede-4fb3-4698-a8f6-684d7786b067&sktid=a48cca56-e6da-484e-a814-9c849652bcb3&skt=2023-04-21T11%3A32%3A50Z&ske=2023-04-22T11%3A32%3A50Z&sks=b&skv=2021-08-06&sig=yUBXEx%2B6T8Snju2PlYYiTwdt559Bbi8vDSn05umSuqI%3D"
            }
        ]
    }
    ```

3. Run the flow manually to see the result. Copy the **Body** from the output of the HTTP request.
4. Now, we will use **Parse JSON** action to extract the output.

    ![](/media/2023-04-23-test-images-spo-openai-dalle-power-automate/03.png)

5. In the next step, let us initialize a variable named `varFileNumber` of type `integer` to name the files with incremental value.

    ![](/media/2023-04-23-test-images-spo-openai-dalle-power-automate/04.png)

6. Using the `Apply to each` action, we will iterate through each image URL generated by OpenAI API DALL-E.

    ![](/media/2023-04-23-test-images-spo-openai-dalle-power-automate/05.png)

7. In the next step, use the `Create file` action of SharePoint to create a file from URL returned by OpenAI DALL-E API. Increment the variable `varFileNumber` to generate incremental value for the file name.

    ![](/media/2023-04-23-test-images-spo-openai-dalle-power-automate/06.png)

    The file name is generated with below formula:

    ```
    concat(triggerBody()['text_1'], '_', variables('varFileNumber'), '.png')
    ```

> Note: You may use RegEx to extract out the actual file name generated from the OpenAI.

## End Result

After running the Power Automate, the sample images will be generated in the SharePoint document library as follows.

![](/media/2023-04-23-test-images-spo-openai-dalle-power-automate/07.png)


## Summary

OpenAI DALL-E API can be used effectively to generate sample images related to any topic. This feature can be combined with Power Automate to generate images in SharePoint.


## References

- [OpenAI](https://openai.com/)
- [DALL·E 2](https://openai.com/product/dall-e-2)
- [OpenAI Images - API Reference](https://platform.openai.com/docs/api-reference/images)
