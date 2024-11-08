Is a broken access control type of vulnerability that uses user supplied input to look for objects and thus having too much trust in user.

IDOR - Insecure direct object reference

Example:
link you click on goes to http://online-service.thm/profile?user_id=1305, and you can see your information.  
Curiosity gets the better of you, and you try changing the user_id value to 1000 instead (http://online-service.thm/profile?user_id=1000), and to your surprise, you can now see another user's information. 
You've now discovered an IDOR vulnerability!

#### Encoded IDs
Often websites use base64 to encode the data, say a given base64 string decodes into {"id":30}.
You can now edit the data and encode it back to base64 string.

#### Hashed IDs
The id being a base 10 decimal number, can sometimes be hashed via md5 algorithm or similar.
It's worthwhile putting the hash in crackstation.net or hashes.com

#### Unpredictable IDs
If the way IDs are used/generated is too unpredictable, creating two user accounts and then trying to swap the ID to the other account as a way to check if you can takeover.

#### Where are IDORs located
The vulnerability may not always be located in the url bar.
Sometimes it can be content your browser loads in via AJAX request or something that you find referenced in the JavaScript file.

Sometimes endpoints could have an unreferenced parameter that may have been of some use during development and got pushed to production. For example, you may notice a call to **/user/details** displaying your user information (authenticated through your session). But through an attack known as parameter mining, you discover a parameter called **user_id** that you can use to display other users' information, for example, **/user/details?user_id=123**.

#### Practical example
The **Your Account** section gives you the ability to change your information such as username, email address and password. You'll notice the username and email fields pre-filled in with your information.  

  

We'll start by investigating how this information gets pre-filled. If you open your browser developer tools, select the network tab and then refresh the page, you'll see a call to an endpoint with the path /api/v1/customer?id=`{user_id}`.

  

This page returns in JSON format your user id, username and email address. We can see from the path that the user information shown is taken from the query string's id parameter (see below image).  

  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/5d71d3fe747a8c8934564feddfc69f75.png)  

  

You can try testing this id parameter for an IDOR vulnerability by changing the id to another user's id. Try selecting users with IDs 1 and 3 and then answer the questions below.****