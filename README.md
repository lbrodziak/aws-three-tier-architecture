# Build a Three-Tier Web App

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-compute-threetier)

**Author:** Łukasz Brodziak  
**Email:** lukasz.brodziak@gmail.com

---

## Build a Three-Tier Web App

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-threetier_2b3c4d5e)

---

## Introducing Today's Project!

In this project, I will demonstrate how to create a fully functional three-tier application.

### Tools and concepts

Services I used were S3, CloudFront. S3, API Gateway, Lambda adn DynamoDB. Key concepts I learnt include Lambda functions, website hosting in CloudFront, API creation and CORS policies.

### Project reflection

This project took me approximately an hour. The most challenging part was fixing permissions. It was most rewarding to see thethree tier application finally working as intended.

I chose to do this project today because I wanted to learn about three-tier architecture and scalabel web app hosting.

---

## Presentation tier

For the presentation tier, I will set up an S3 bucket that will store files needed for the website to work. Next I will setup Cloudfront CDN to host the website stored on S3.
Detailed description of this part can be found [here](https://github.com/lbrodziak/aws-devops-cloudfront)

I accessed my delivered website by using the Distribution domain name link.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-threetier_3a4b5c6d)

---

## Logic tier

For the logic tier, I will [set up the logic tier](https://github.com/lbrodziak/aws-devops-api-and-lambda) consisting of Lambda funtion and API Gateway.

The Lambda function retrieves data by utilizing functions from Dynmo DB SDK.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-threetier_6a7b8c9d)

---

## Data tier

For the data tier, I will [set up the Dynamo DB](https://github.com/lbrodziak/aws-devops-dynamo-db) table to store user data to be fetched by Lambda function

A partition key is the heart of how DynamoDB organizes data. Think of it as a label that you can use to group similar items. Under the hood, the partition key is how DynamoDB spreads out your data across different servers for quick access and efficient querying.

Every item in your table must have a unique partition key.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-threetier_u1v2w3x4)

---

## Logic and Data tier

Once all three layers of my three-tier architecture are set up, the next step is to integrate them so that they work together to fetch the user data and present it on the web page

To test my API, I went to invoke URL of the API and added  /users?userId=1/ As a result I saw DynamoDb data returned in JSON format

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-threetier_a112c3d5)

---

## Console Errors

The error in my distributed site was because the JavaScript file does not contain the URL of the API.

To resolve the error, I updated script.js by replacing placeholder with proper API URL. I then reuploaded it into S3 because I have to update the file so it will ne properly hosted o CloudFront.

I ran into a second error after updating script.js. This was an error with fetching data because of the request being blocked by CORS policy.
CORS (Cross-Origin Resource Sharing) is like a security bouncer for your browser. It decides whether your frontend (like your CloudFront-hosted site) is allowed to talk to a backend server (like API Gateway).

By default, browsers are super cautious - they don’t let one website access resources from another domain (e.g. your frontend trying to call your API Gateway) unless the backend explicitly says it's okay.

In this case, because CORS isn’t configured in API Gateway, the browser freaks out and blocks the request from CloudFront, giving you those annoying CORS errors.

So, we need to enable CORS on your API Gateway. This is like handing the browser a VIP list that says, "Yep, the CloudFront URL is trusted - let it through."

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-threetier_a1b2c3d5)

---

## Resolving CORS Errors

To resolve the CORS error, I first enabled CORS on the /users resource in my API. Then I set the CloudFront URL as Allow-Origin.

I also updated my Lambda function. The changes I made were adding CORS to the code. This due to the fact that I set proxy integration.
When proxy integration is enabled, API Gateway simply forwards the request to your Lambda function and expects the Lambda to return the full HTTP response, including the CORS headers.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-threetier_1qthryj2)

---

## Fixed Solution

I verified the fixed connection between API Gateway and CloudFront by goin to CloudFront distribution URL and entering 1 in the User Id field. After clicking the button the data was successfully fetched from DynamoDB and presented as JSON.

![Image](http://learn.nextwork.org/surprised_maroon_fierce_chinese_gooseberry/uploads/aws-compute-threetier_2b3c4d5e)

---

---
