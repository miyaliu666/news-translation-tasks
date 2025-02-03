---
title: How to Build an Application with AWS Lambda
date: 2025-02-03T12:50:57.535Z
author: Ijeoma Igboagu
authorURL: https://www.freecodecamp.org/news/author/Ijay/
originalURL: https://www.freecodecamp.org/news/how-to-build-an-application-with-aws-lambda/
posteditor: ""
proofreader: ""
---

AWS Lambda is a service from Amazon Web Services (AWS) that lets you run your code in response to events without managing servers. It’s a simple and scalable way to build applications.

<!-- more -->

In this tutorial, I’ll show you how to use AWS Lambda with three other services:

-   **Amazon S3** for storing files, images, and videos
    
-   **Amazon Simple Notification Service (SNS)** for sending notifications
    
-   **Amazon EventBridge** for scheduling messages
    

We’ll go through everything step by step.

By the end, with the integration of the other services, you will have built a Goal Manifestation Quote App that sends random inspirational messages to keep you motivated and focused on your goals.

### Prerequisites

-   An AWS account: If you don’t have one, sign up [here][1].
    
-   A GitHub repository: This is for storing your source code. If you don’t have a GitHub account, you can create one [here][2].
    
-   An Integrated Development Environment (IDE) such as [Visual Studio Code][3] or [Sublime Text][4].
    
-   A basic knowledge of web development and any programming language of your choice. I used Python for this tutorial.
    
-   [Zenquote Random API][5]
    

### What You'll Learn

-   How to create an Amazon S3 bucket
    
-   How to use Amazon Simple Notification Service (SNS)
    
-   How to use Amazon Lambda
    
-   How to use Amazon EventBridge
    

## **Table of Contents**

1.  [Step 1: Set Up Your Development Environment][6]
    
2.  [Step 2: Create an Amazon Simple Storage Service (S3)][7]
    
3.  [Step 3: Create an Amazon Simple Notification Service (SNS)][8]
    
4.  [Step 4: Create an IAM Policy][9]
    
5.  [Step 5: Create an Amazon Lambda function][10]
    
6.  [Step 6: Create an EventBridge][11]
    
7.  [Step 7. Upload Your Code][12]
    
8.  [Conclusion][13]
    

![Let's get started 🚀](https://cdn.hashnode.com/res/hashnode/image/upload/v1736791948488/38dfe402-1050-410d-869b-0cef2797b792.png)

## Step 1: Set Up Your Development Environment

In this step, you'll get everything set up. Start by signing in to your AWS account, then install [Python][14] if you don't have it on your IDE.

## **Step 2: Create an Amazon Simple Storage Service (S3)**

Before we begin creating an S3 bucket, let's first understand what Amazon S3 is:

**Amazon S3 (Simple Storage Service)** is a service from Amazon that allows you to store and access any amount or type of data, such as photos, videos, documents, and backups, whenever you need it.

Now that you know the basics of what Amazon S3 is, let's return to the tutorial.

### Create an S3 Bucket

There are several ways to create an S3 bucket, but for this tutorial, we'll use the [Ubuntu command line (CMD)][15], your terminal, or **Amazon CloudShell**, depending on what you're most comfortable with.

-   Type boto3 s3 in the web search bar to view a list of related documentation.
    
-   Click on the first result.
    

![Regular Google search](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792137101/5f38b4ec-fa23-41b3-b108-ca7fc7b390ba.png)

-   Once the documentation is open, copy the first command you see.

![boto3 command](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792202800/5647c731-734f-4134-a558-9d66eee47734.png)

-   Paste it on your CMD OR terminal of your choice – but before then remember to "**cd**" into the right directory.

![paste command from documentation to your editor](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792298332/d3384fc3-e31c-4d37-8e17-40ad7e77df28.png)

-   In the documentation, scroll down and click on "create\_bucket.

![0cd59a14-b037-464b-8193-7ec515c4772e](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792399748/0cd59a14-b037-464b-8193-7ec515c4772e.png)

-   Once it's open, scroll down to "Request Syntax." Copy the **bucket name** and the **bucket configuration**.
    
-   Other variables listed in the request syntax are optional.
    

![Request syntax from the documentation](https://cdn.hashnode.com/res/hashnode/image/upload/v1736792898846/eea0f8c4-d153-4bc8-8c78-346fc5bf6a04.png)

-   Once this is done, make sure you save.

![All command](https://cdn.hashnode.com/res/hashnode/image/upload/v1736793004865/a1c9739c-2d12-4e2d-b057-f09bc61e16a3.png)

-   Go back and call the script:

```
#python3 your file name
```

-   Running the script automatically creates an S3 bucket in your Amazon S3.

![Automatic creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1736793086405/9c0b4671-ea07-4ad7-b1d7-0d785aafa954.png)

-   Now you can go to the console to check if it has been created:

![Amazon console](https://cdn.hashnode.com/res/hashnode/image/upload/v1736692453693/320318d4-bdf3-4be3-a709-6cff18459c9c.png)

### Upload Files

With the bucket created, we can now upload files through the console. I believe there's also a programmatic way to upload files and test, but I haven't explored all the methods in the documentation yet.

Click on the bucket name to be redirected to the object page. This is where you will upload your files for storage.

![upload page](https://cdn.hashnode.com/res/hashnode/image/upload/v1736692660862/355f828f-f83f-4501-a960-f0068cf9d977.png)

Click on the **Upload button** to upload a file. Remember, we're creating a Goal Manifestation Quote Application.

Now that we've set up a storage bucket:

-   Open a tool like Google Drive, MS Word, WPS, or any other document editor.
    
-   Write down the goals you want to achieve.
    
-   Save the file in either PDF or DOCX format.
    
-   Take the document and upload it to your Amazon S3.o
    

![upload a document](https://cdn.hashnode.com/res/hashnode/image/upload/v1736693525955/765b6c3a-ae68-4cbc-9df7-e896a1d63cbb.png)

To verify if it's the correct file:

-   Navigate to the **Permissions** tab.
    
-   Scroll down to **Block public access.**
    
-   Click on **Edit** and uncheck the box.
    

![block access ](https://cdn.hashnode.com/res/hashnode/image/upload/v1736738796036/6ab41bc4-72a8-4874-a491-35bcdda49938.png)

As shown above, it is currently set to "on." Uncheck it to turn it "off."

-   On the same bucket settings page, modify the policy.
    
-   Scroll down, and you'll see that a bucket policy has been auto-generated.
    
-   Go ahead and copy the policy.
    

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
```

-   Go back to the bucket policy editor and paste the policy.

Once you've completed these steps, your object will have public access.

Return to the **Objects** tab and click on the Object URL provided below:

![3b36b380-912d-4a2a-a8c5-bee61bd42765](https://cdn.hashnode.com/res/hashnode/image/upload/v1736740454730/3b36b380-912d-4a2a-a8c5-bee61bd42765.png)

With this URL, your upload is now visible.

## **Step 3: Create an Amazon Simple Notification Service (SNS)**

**SNS** is a fully managed messaging service provided by AWS. It enables communication between applications or directly with users by sending notifications.

To create an SNS, follow these steps:

#### **1\. Log in to the AWS management console**

Then go to Amazon SNS. Navigate to the SNS Dashboard and select **Topics** from the left-hand menu.

To create a topic:

-   Click **Create topic**.
    
-   Choose a **Topic type**: Standard (default) or FIFO (for ordered messages).
    
-   Enter a **Name** for your topic. (for example, `MyFirstSNSTopic`).
    
    ![sns topic creation](https://cdn.hashnode.com/res/hashnode/image/upload/v1736743311856/fa51ecb9-935a-4567-829c-b84ee3c1bee0.png)
    
-   Configure optional settings like encryption, delivery retry policies, or tags.
    
-   Click **Create topic**.
    

#### **2\. Add Subscriptions:**

Once the topic is created, click on it to open the details page. Select the **Subscriptions** tab.

Click **Create Subscription** and choose:

-   **Protocol** can be Email, SMS, HTTP/S, Lambda, or SQS.
    
-   **Endpoints** such as email address, phone number, or URL.
    

Click **Create Subscription**.

![Subscription created](https://cdn.hashnode.com/res/hashnode/image/upload/v1736743374208/8005dc71-68ed-4d26-b79b-7b418959ab6c.png)

#### **3\. Confirm the Subscription:**

If you selected email or SMS, a confirmation link or code will be sent to the provided endpoint. Follow the instructions to confirm the subscription.

![A confirm message from amazon sns](https://cdn.hashnode.com/res/hashnode/image/upload/v1736743525222/90c3b61d-eeb2-450d-a397-253b9e3c15db.png)

Now that we’ve done this, let's create an Amazon Lambda function that will trigger the SNS so that the message will be sent to your mail.

## **Step 4: Create an IAM Policy**

This policy is created to authorize Amazon Lambda to trigger the event and to ensure that CloudWatch is automatically triggered to monitor the application's events.

To create a policy, follow these steps:

#### **1\. Log in to the AWS Management console.**

In the left-hand menu, select **Policies**. Then:

-   Click **Create policy**.
    
-   Choose the **Visual** tab, then select the **SNS** service.
    
-   Next, click the **Choose** tab to create a custom policy.
    

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sns:Publish",
            "Resource": "arn:aws:sns:REGION:ACCOUNT_ID:goal_topic"
        }
    ]
}
```

Then, replace the following placeholders with your info:

-   `region`: Your AWS region (for example, `us-east-1`).
    
-   `account-id`: Your AWS account ID.
    
-   `topic-name`: Your SNS topic name.
    

#### **2\. View and create the policy:**

You can do this by following these steps:

-   Click on the Review button.
    
-   Give your policy a **Name** (for example, `LambdaSNSPolicy`), and optionally, a **Description**.
    
-   Click **Create policy**.
    

#### 3\. Attach policy to the Lambda Execution Role

Now, you need to attach the policy to your Lambda Execution Role. To do that, follow these steps:

-   Go to the **Roles** section in the IAM Console.
    
-   Search for and select the execution role.
    

![lambdaexecution role](https://cdn.hashnode.com/res/hashnode/image/upload/v1736749886571/70ea6752-5c13-465c-bc05-22e3da50127e.png)

-   Next, search for the policy you just created and select it.
    
-   Click **Attach policy**.
    

Both policies will be automatically attached.

## **Step 5: Create an Amazon Lambda function**

Amazon Lambda is a service from AWS that lets you run code without managing servers. You upload your code, and Lambda automatically runs and scales it when needed.

Follow these steps to create an Amazon Lambda function:

#### **1\. Log in to AWS Management Console**:

Navigate to AWS Lambda.

#### **2\. Create a Function**:

Click on the **Create function** and choose the option **Author from scratch**.

Fill in the details:

-   **Function name**: Enter a unique name (for example, `SNSLambdaFunction`).
    
-   **Runtime**: Select the runtime (for example, Python, Node.js, Java, and so on).
    

![creating a function](https://cdn.hashnode.com/res/hashnode/image/upload/v1736751631488/fe254e56-89f1-4938-b2a9-e59b24e54a04.png)

-   **Role**: Choose or create a role. If you already have a role, select **Use an existing role**. Otherwise, select **Create a new role with basic Lambda permissions**.

![role choosing](https://cdn.hashnode.com/res/hashnode/image/upload/v1736751816631/97475623-10dc-4427-87ab-7e8789526e9e.png)

-   Click the **Create function button**.

#### **3\. Paste in the code**:

On the Lambda function’s page, go to the **Configuration** tab:

![configuration tab](https://cdn.hashnode.com/res/hashnode/image/upload/v1736753706928/b0351665-c506-454e-9c71-b4a52fce5a0a.png)

Remember, we are trying to fetch a quote. I'll add the ARN of the topic we created here and include my API keys. But for this tutorial, I will use the API directly to fetch the data.

![1322e207-5c51-45f1-979f-05f2388e9557](https://cdn.hashnode.com/res/hashnode/image/upload/v1736754071465/1322e207-5c51-45f1-979f-05f2388e9557.png)

#### **4\. Write the Lambda Code:**

Go to the **Code** tab in your Lambda function. Then write or paste the code from your IDE to process the incoming SNS messages.

Example:

![Testing code](https://cdn.hashnode.com/res/hashnode/image/upload/v1736754319755/60224578-53c2-43f6-9bf1-8cba6afaa1a3.png)

Here’s the code:

```
import os
import json
import urllib.request
import boto3

def fetch_random_quote():
    """
    Fetches a random quote from the ZenQuotes API.
    """
    api_url = "https://zenquotes.io/api/random"
    try:
        with urllib.request.urlopen(api_url) as response:
            data = json.loads(response.read().decode())
            if data and isinstance(data, list):
                # Format the quote and author
                quote = data[0].get("q", "No quote available")
                author = data[0].get("a", "Unknown author")
                return f'"{quote}" - {author}'
            else:
                return "No quote available."
    except Exception as e:
        print(f"Error fetching random quote: {e}")
        return "Failed to fetch quote."

def lambda_handler(event, context):
    """
    AWS Lambda handler function to fetch a random quote and publish it to an SNS topic.
    """
    # Get the SNS topic ARN from environment variables
    sns_topic_arn = os.getenv("SNS_TOPIC_ARN")
    sns_client = boto3.client("sns")

    # Fetch a random quote
    quote = fetch_random_quote()
    print(f"Fetched Quote: {quote}")

    # Publish the quote to SNS
    try:
        sns_client.publish(
            TopicArn=sns_topic_arn,
            Message=quote,
            Subject="Daily Random Quote to help you stay motivated and inspired to achieve your goals",
        )
        print("Quote published to SNS successfully.")
    except Exception as e:
        print(f"Error publishing to SNS: {e}")
        return {"statusCode": 500, "body": "Error publishing to SNS"}

    return {"statusCode": 200, "body": "Quote sent to SNS"}
```

#### **5\. Save:**

Click on the deploy button to save.

![8ec83b6b-874c-47c3-a1ad-8d8b85cd6d48](https://cdn.hashnode.com/res/hashnode/image/upload/v1736756768348/8ec83b6b-874c-47c3-a1ad-8d8b85cd6d48.png)

#### **6\. Test Your Lambda Function**:

Go to the **Test** tab and create a new test event.

![TEST EVENT](https://cdn.hashnode.com/res/hashnode/image/upload/v1736757103169/834c5b6c-8796-4a52-bcc3-cbd8009b01da.png)

Then save and run the test. If it’s successful, a message will be sent:

![success msg](https://cdn.hashnode.com/res/hashnode/image/upload/v1736757309807/d97f9648-0000-4ec8-b287-df5250b3be0a.png)

This means the message has been created for you

Finally, check your email or SMS, depending on the endpoint you used for this tutorial. In my case, I used email.

![email notification](https://cdn.hashnode.com/res/hashnode/image/upload/v1736757884384/d5b949d4-0804-4694-9674-77fc0265e2e8.png)

## **Step 6: Create an EventBridge**

Amazon EventBridge is a service that helps you connect applications and AWS services such as the Amazon SNS and Amazon Lambda.

To create an Amazon EventBridge rule, follow these steps:

#### **1\. Navigate to EventBridge**:

In the search bar, type **EventBridge** and select it from the services list.

#### **2\. Create a Rule:**

In the EventBridge console, click **Rules** on the left panel. Then click the **Create rule** button.

#### **3\. Set Up the Rule Details**:

-   **Name**: Enter a unique name for your rule.
    
-   **Description (optional)**: Add a description to explain what this rule does.
    

#### **4\. Choose the Event Bus**:

Select **Default event bus** (or another event bus if you've created one).

#### **5\. Define the Event Pattern or Schedule**:

**For Event Pattern**:

-   Choose an **AWS Service** as the event source.
    
-   Select the specific **event type** (for example, an S3 file upload or an EC2 instance state change).
    

**For Schedule**:

-   Choose the **Schedule** option to run the rule on a fixed interval (for example, every 5 minutes).

![rule details](https://cdn.hashnode.com/res/hashnode/image/upload/v1736759371221/ca28dcf2-061a-4c8f-8191-32bdf8380111.png)

-   Click on continue. This takes you to the specific details page where:

![schedule page](https://cdn.hashnode.com/res/hashnode/image/upload/v1736759696183/44ba44e8-2a1b-4cd8-928b-6fe87cc35c4e.png)

-   Scroll down and click on the cron scheduler. The cron scheduler specifies what time the message will be sent.
    
-   Select **"Off"** for the flexible time window option.
    
-   Review the rule details to confirm everything is correct.
    
-   Click the **"Next"** button to proceed to the **Target** page.
    
    ![cron scheduler ](https://cdn.hashnode.com/res/hashnode/image/upload/v1736760213450/c4f6cccf-7f89-485d-802e-051284c89f9a.png)
    
    The picture above shows when the time the messages will be sent.
    
    -   On the Target page, select **AWS Lambda** to invoke your function.

![the target page](https://cdn.hashnode.com/res/hashnode/image/upload/v1736761288843/e35a7153-48d7-4b34-86ce-9cba68bd5161.png)

-   Scroll down to invoke and choose the function you created.

![invoke the function](https://cdn.hashnode.com/res/hashnode/image/upload/v1736761539389/4faa1dbc-5635-4de3-8087-15a444298b2e.png)

-   Click the "Next" button to proceed. This will take you to the settings page. Under the permissions section, select "Use existing rule."

![setting page](https://cdn.hashnode.com/res/hashnode/image/upload/v1736782488383/ffbbcc4e-6379-4c9c-af16-f4c017e7426c.png)

-   Lastly, go to the review and create a schedule:

![2d0e7ff9-bb7d-462d-a644-23517f47e53e](https://cdn.hashnode.com/res/hashnode/image/upload/v1736783029778/2d0e7ff9-bb7d-462d-a644-23517f47e53e.png)

-   The next page shows you all the details:

![4545d2da-5020-46c3-a265-5499e6b4e74f](https://cdn.hashnode.com/res/hashnode/image/upload/v1736783558777/4545d2da-5020-46c3-a265-5499e6b4e74f.png)

Using the EventBrige creates a scheduler for the users.

## **Step 7: Upload Your Code**

Finally, upload your code to GitHub and include proper documentation to help explain how the code works.

Check this documentation out if you don’t know how: [Uploading a project to GitHub][16].

## Conclusion

If you’ve followed all these steps, you will have created a Goal Manifestation Quote App using AWS Lambda, Amazon S3, Amazon SNS, and Amazon EventBridge. This app fetches motivational quotes and sends them to subscribers on a schedule.

You can find the repository link [here][17].

Feel free to share your progress or ask questions if you have any issues.

If you found this article helpful, share it with others.

Stay updated with my projects by following me on [Twitter][18], [LinkedIn][19] and [GitHub][20]

Thank you for reading 💖.

**Disclaimer:**  
The resources shown in this article, including the S3 bucket and its ARN, have been deleted and no longer exist. The details visible in the screenshots are used solely for demonstration purposes.

[1]: https://aws.amazon.com/
[2]: https://github.com/
[3]: https://code.visualstudio.com/
[4]: https://www.sublimetext.com/download
[5]: https://zenquotes.io/
[6]: #heading-step-1-set-up-your-development-environment
[7]: #heading-step-2-create-an-amazon-simple-storage-service-s3
[8]: #heading-step-3-create-an-amazon-simple-simple-notification-service
[9]: #heading-step-4-create-an-iam-policy
[10]: #heading-step-5-create-an-amazon-lambda-function
[11]: #heading-step-6-create-an-eventbridge
[12]: #heading-step-7-upload-your-code
[13]: #heading-conclusion
[14]: https://www.python.org/downloads/
[15]: https://ubuntu.com/download
[16]: https://docs.github.com/en/get-started/start-your-journey/uploading-a-project-to-github
[17]: https://github.com/ijayhub/goal-manifestation-quote
[18]: https://https//twitter.com/ijaydimples
[19]: https://www.linkedin.com/in/ijeoma-igboagu/
[20]: https://github.com/ijayhub