# Storing Data in Amazon Simple Storage Service (Amazon S3)

All big data solutions begin with storing data. This is the first step in the *big data pipeline*. You can store data with several different services from Amazon Web Services (AWS). Amazon Simple Storage Service (Amazon S3) is one of the most commonly used services for storing data.

In this lab, you will practice using the AWS Management Console to create an S3 bucket. You will then add an AWS Identity and Access Management (IAM) user to a group that has full access to Amazon S3. You will also upload files to Amazon S3, and run simple queries on the data in Amazon S3.

<img src="images/lab1.png" alt="architectural diagram" width="800">

For more information about Amazon S3, refer to the <a href="https://docs.aws.amazon.com/s3/index.html?id=docs_gateway#lang/en_us" target="_blank">Amazon Simple Storage Service documentation</a>.

# Objectives

After completing this lab, you will be able to:

- Access Amazon S3 in the AWS Management Console
- Secure an S3 bucket with IAM
- Create a bucket with Amazon S3
- Load data into an S3 bucket
- Query an S3 bucket


**Prerequisites**

This lab requires:

- Access to a notebook computer with Wi-Fi and Microsoft Windows, macOS, or Linux (Ubuntu, SuSE, or Red Hat)
- For Microsoft Windows users: Administrator access to the computer
- An internet browser such as Chrome, Firefox, or IE9 (previous versions of Internet Explorer are not supported)

**Duration**

This lab requires **30** minutes to complete. The lab will remain active for **60** minutes to allow extra time if needed.

## Accessing the AWS Management Console

1. At the top of these instructions, choose <span id="ssb_voc_grey">Start Lab</span> to launch your lab.

    A **Start Lab** panel opens, which displays the lab status.

2. Wait until you see the message *Lab status: ready*, then close the **Start Lab** panel by choosing the **X**.

3. At the top of these instructions, choose <span id="ssb_voc_grey">AWS</span>

  This will open the AWS Management Console in a new browser tab. The system will automatically log you in.

  **Tip**: If a new browser tab does not open, there will typically be a banner or icon at the top of your browser that indicates that your browser is preventing the website from opening pop-up windows. Choose the banner or icon and then choose **Allow pop ups**.

4. Arrange the **AWS Management Console** browser tab so that it displays next to these instructions. Ideally, you should be able to see both browser tabs at the same time, which can make it easier to follow the lab steps.


## Task 1: Create an IAM user account

You must have permissions to access Amazon S3. IAM is a web service for securely controlling access to AWS services. One best practice for managing IAM permissions is to create groups of users with a set of permissions. These permissions are controlled by IAM policies. An IAM policy is an entity that you attach to identities or resources to define permissions. To learn more about IAM identities and policies, refer to <a href="https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html" target="_blank">Policies and Permissions</a>.

In this task, you will review the permissions for the *awsusers* IAM group and add the *awsuser* to that group.

  **Note**: In this lab, the *awsusers* group and *awsuser* user were created for you. In a real-world scenario, an administrator would create this group and manage adding users to the group.

### Task 1.1: Review users and group permissions in the IAM console

In the task, you will create a new group of user accounts

5. On the AWS Management Console, on the **Services** menu, choose **Services**.

6. From the list of services, choose **IAM**.

7. In the navigation pane, choose **Groups**.

8. Choose the **awsusers** group.

9. Choose the **Permissions** tab.

  Notice that the *AmazonS3FullAccess* policy is attached to the group.

10. Choose **Show Policy**

   The policy document is in JavaScript Object Notation (JSON) format. This policy states that users in that group are allowed to take all actions for Amazon S3 on all resources.

11. Choose **Cancel**.

12. In the **Inline Policies** section, choose **Show Policy**.

  The policy document is in JSON format. This policy states that users in the group are not allowed to perform the following specified actions on S3 objects:

  - *ObjectLegalHold* – A legal hold prevents an object version from being overwritten or deleted.
  - *ObjectRetention* – A retention period determines how long an object is retained.
  - *BucketObjectLock* – When an object is locked, it cannot be deleted or overwritten.

13. Choose **Cancel**.

### Task 1.2: Add awsuser to the awsusers group

In this task, you will add the *awsuser* to the *awsusers* group. You will also log out of the console and log back in to the console with the *awsuser* account and password.

14. In the navigation pane, choose **Groups**.

15. Select the **awsusers** group.

16. From the **Group Actions** menu, choose **Add Users to Group**.

17. Select the **awsuser** user.

18. Choose **Add Users**.

19. From the navigation header, open the list of account actions and copy the account ID.

<img src="images/account.png" alt="Account information" width="400">

20. In the list of account actions, choose **Sign Out**.

21. To sign back in with the *awsuser* credentials, choose **Sign In to the Console**.

22. Select **IAM user** and then use the following information to sign in:

  **Note**: Remove the dashes from the account number before you enter it.

  - Account: The account ID that you previously copied
  - IAM user name: `awsuser`
  - Password: `myP@ssW0rd`


## Task 2: Load data into Amazon S3

Buckets and objects are the basic building blocks for Amazon S3. You create buckets and add objects to the buckets. Objects in Amazon S3 can be up to 5 TB.

### Task 2.1: Create an S3 bucket

In this task, you will create a new S3 bucket.

23. On the AWS Management Console, on the **Services** menu, choose **Services**.

24. From the list of services, choose **S3**.

25. Choose **Create bucket**.

26. Enter a bucket name with three or more characters. Uppercase characters are not allowed.

27. Choose **Create bucket**.

  **Note**: S3 bucket names must be unique across all buckets in Amazon S3. If you get a conflict with another bucket, add a digit and try again.

  **Note**: Write down the bucket name because it will be used in future steps.

### Task 2.2: Upload an object

In this task, you will upload an object to the S3 bucket that you created. First, you must <a href="%% S3_HTTP_PATH_PREFIX %%/lab1.csv" target="blank">get the file</a>.

28. Download the lab1.csv file to a local directory.

29. Choose the bucket that you created in the previous task.

30. In the Amazon S3 console, choose **Upload**.

<img src="images\fileupload.png" alt="Amazon S3 Console" width="900">

31. Choose **Add files**.

32. Browse to the directory where you stored the lab1.csv file.

33. Choose the **lab1.csv** file.

34. Choose **Upload**.

### Task 2.3: Query the object you uploaded

In this task, you will query the object that you uploaded to verify that it was uploaded successfully.

35. In the Amazon S3 console, choose the **lab1.csv** file.

36. Review the file properties for the file that you uploaded.

  **Note**: You should get a message stating that versioning is not enabled for the bucket. This behavior is expected. 

37. From the **Object actions** menu, choose **Query with S3 Select**.

38. Scroll down the page and choose **Run SQL query**.

  You should see the first few records from the file.

<img src="images\queryresults.png" alt="Query results" width="900">  

39. Choose **Add SQL from templates**.

40. Choose **SELECT COUNT * FROM s3object s**.

41. Choose **Copy SQL**.

42. Replace the previous query by deleting it and then paste the query you copied.
  
43. Choose **Run SQL query**.

In the **Result** pane, you should get the total number of records, which is *5*.

### Task 2.4: Change the encryption properties and storage type

You can set individual object properties—such as encryption at rest and storage class type—in the Amazon S3 console. Amazon S3 supports two kinds of encryption: Advanced Encryption Standard (AES)-256, and AWS Key Management Service (AWS KMS).

If you select server-side encryption, each object has a unique key. The keys are also encrypted with a master key that AWS rotates regularly. If you choose to use AWS KMS, your objects will also be encrypted with unique keys, but you will manage those keys yourself.

When you uploaded the lab1.csv file, you accepted the default storage class, which is *Standard*. Amazon S3 provides six different storage classes, each with different properties and cost structures. For more information about Amazon S3 storage classes, refer to <a href="https://aws.amazon.com/s3/storage-classes/" target="blank">Storage Classes</a>.

In this task, you will change the encryption setting and storage class for the lab1.csv file.

44. In the Amazon S3 breadcrumbs, choose the bucket name for your bucket.

45. In the Amazon S3 console, choose the **lab1.csv** file.

46. From the **Object actions** menu, choose **Edit server-side encryption**.

47. Choose **Enable** and **Save changes**.

48. To return to the object overview page, choose **Exit**.

49. From the **Object actions** menu, choose **Edit storage class**.

50. Select **Intelligent-Tiering** and **Save changes**.

You receive a confirmation that you successfully edited the storage class.

### Task 2.5: Upload a compressed file

For big data scenarios, you should generally store compressed files in Amazon S3. Amazon S3 supports the .gzip and .bzip2 compression formats. Uploading a compressed file is essentially the same as uploading a file that is notebook compressed.

In this task, you will upload a file that is compressed as a .gzip file. First, you must <a href="%% S3_HTTP_PATH_PREFIX %%/lab1.csv.gz" target="blank">get the file</a> and save it to a local directory.

51. In the Amazon S3 console, choose your bucket from the breadcrumbs again.

52. Choose **Upload**.

53. Choose **Add files**, and choose the **lab1.csv.gz** file that you downloaded previously.

54. Choose **Upload**.

55. Select the **lab1.csv.gz** file.

56. To close the **Upload: status** page, choose **Exit**.

57. From the **Object actions** menu, choose **Query with S3 Select**.

58. Scroll down the page and choose **Run SQL query**.

You should get results that demonstrate that you can query the compressed file in the same way as a non-compressed file.

## Lab complete

<i class="icon-flag-checkered"></i> Congratulations! You have completed the lab.

59. At the top of this page, choose <span id="ssb_voc_grey">End Lab</span> and then confirm that you want to end the lab by choosing <span id="ssb_blue">Yes</span>

   A panel will appear, with this message: *DELETE has been initiated... You may close this message box now.*

60. To close the panel, go to the top-right corner and choose the **X**.



*©2020 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.*