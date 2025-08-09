[001]: img/create%20bucket-1.png "Title"
[002]: img/create%20bucket-2.png "Title"
[003]: img/view%20bucket.png "Title"
[004]: img/upload%20files-1.png "Title"
[005]: img/upload%20files-%20succeeded.png "Title"
[006]: img/static%20web%20hosting-1.png "Title"
[007]: img/static%20web%20hosting-2.png "Title"
[008]: img/s3-%20403%20forbidden.png "Title"
[009]: img/cloudfront-%20distributions%20list.png "Title"
[010]: img/cloudfront-%20website%20error%20page.png "Title"
[011]: img/cloudfront-%20edit%20settings.png "Title"
[012]: img/ready%20website.png "Title"


# Host a Static Website On Amazon S3 and Setup DNS With Amazon CloudFront
### By Paul Onyebuchi

## About this project:
In this project, I setup a static website on AWS S3. Amazon S3 is an object storage service offered by AWS to provide highly scalable, durable and available platform for storing and retrieving any amount of data from anywhere in the world. It is just the perfect tool for hosting our static website. The content files I use here will be HTML, CSS, Javascript and image files from my first ever portfolio page written when I was just learning how to code.

This project also incorporates Amazon CloudFront as a content delivery network (CDN) to serve our website content by caching content closer to global viewers, reducing redundancy and improving our website’s performance. CDNs work by storing copies of website content on servers closer to users. AWS has its servers around the world in strategically located data centers known as Edge Locations.


At the end of this project, we will have a static website that is highly scalable, fast and secure.

## AWS services used in this project
Simple storage service (S3) – For storing and hosting our web contents.

CloudFront – A content delivery network (CDN) by Amazon Web Services (AWS)

### Overview of Steps
1.	[Write and prepare our website files](#write-and-prepare-our-website-files)
2.	[Create an S3 bucket](#create-an-s3-bucket)
3.	[Upload website files](#upload-website-files-to-our-s3)
4.	[Configure static web hosting](#configure-static-web-hosting)
5.	[Create CloudFront distribustion](#create-cloudfront-distribution)
6.	[Testing](#testing)
7. [Conclusion](#conclusion)

… and so we begin

### Write and prepare our website files
Amazon S3 is suitable for hosting static websites, which consist of contents like html, css and javascript. It does not natively support server-side processing or databases required for dynamic websites unless in conjunction with other AWS services.
I have an already prepared folder of files from an old project, so I’ll be using that here.

### Create an S3 bucket
It took me about 2 minutes to create this bucket, which goes to show how convenient and efficient cloud solutions can be.
I picked the northern California region to provision my S3 bucket because it is the closest to me, since I live in California. When working with AWS services, it is important to provision your services in regions that are closest to your users.
1.	Go to the Amazon S3 console and click ‘***Create bucket***’
2.	Enter a unique bucket name in the field provided for ‘Bucket name’.

*S3 bucket names are globally unique1 because all amazon s3 bucket names share a single, global namespace across all aws accounts and regions. Our bucket names must be as unique as possible*

3.	For object ownership, we will leave it as ***ACLs disabled***.

![Create bucket][001]



*Access Control Lists (ACLs) are an AWS legacy way of controlling ownership of objects written to an S3 bucket. For the security of our web content, we will leave that as disabled. AWS recommends this too.*

4.	For ‘Public access settings for this bucket’, I checked the ‘***Block all public access***’ button.

*Ordinarily, to make our website contents accessible through the internet, we would be required to not check this box, but we want to create a secure website. So, I’m going to leave this box checked and later grant access to only CloudFront.*


5.	***Enable bucket versioning*** if you want S3 to store multiple versions of bucket objects after every update instead of deleting older versions of your files. This will help with easilily recovering from application failures and unintended user actions. I will enable this.

![Create bucket][002]

6.	We will leave the rest of the settings ‘***Default encryption***’ and ‘***Advanced settings***’ as is and just scroll to the page to ***Create bucket***.

![View bucket][003]


And there you go, we have just created our S3 bucket. You can view your new bucket if you go to back to the S3 console and on the left pane click on ***General purpose buckets***.


### Upload website files to S3
Time to upload our prepared web content files to the S3 bucket. Note that the index page of our website must be named index.html. A website’s index page is the HTML document of the website’s home page.
1.	From our S3 general purpose buckets list, click on the bucket name, i.e the link under the names column of our list. This will take you to the objects page of our bucket.
2.	Click on the ‘***Upload***’ button, and on the upload page, we can drag and drop all the files from our local computer to the field provided.

![upload content][004]

3.	Click the ***upload*** button and continue to the next step.
You should get an upload succeeded alert. Review the list of your uploaded files and click the ***close*** button to go back to our objects list page.

![upload successful][005]

Also review the tabs above our objects list. The tabs are objects, properties, permissions, metrics etc.

### Configure static web hosting
Website hosting is what makes our website available on the internet for anyone who has the domain address to be able to view our files. Without hosting, our website is just another bunch of files locked up somewhere in the cloud and no one can see them.

To enable website hosting on our s3 bucket…
1.	While inside our bucket, click on the ***Properties*** tab.
2.	Scroll all the way down to the ***Static website hosting*** section and click on the edit button.
3.	Under static website hosting, select the radio button for ***Enable***.
4.	In the field provided for index document, type in *index.html*.

![Enable static web hosting][006]

5.	Click on ***Save changes*** button.

We have successfully enabled static website hosting on the S3 bucket. AWS will generate a *Bucket website endpoint*. 
The bucket website endpoint is the URL created by AWS through which our static website can be accessed on the internet.

![Bucket website endpoint][007]

6.	Next, copy the bucket website endpoint and paste in a new browser tab.

What do you see? I see a 403 forbidden telling us access to the S3 contents is denied. 

![403 Forbidden][008]

But no worries! We turned off public access to our bucket, so no one can view our files directly from S3. We want a secure and robust solution. Therefore, we will leave public access turned off and give CloudFront permission to serve our web contents instead.

So, lets head over to Amazon CloudFront and implement this ….


## Create a CloudFront distribution
1.	Go to cloudfront console -> Click ‘***Create distribution***’
2.	Enter a name for your distribution, and select ***Single website or app*** under *Distribution type*.
3.	Click ***Next*** to move to the next step.
4.	Under *origin settings*;

*Origin domain*: select ***Amazon S3***
*Origin path*: leave blank
*S3 origin*: Click ***Browse S3*** and select the s3 bucket created earloer from the list of buckets.

*At this point, disregard any AWS prompt recommending to use website endpoint.*

*Settings*: Select *Allow private S3 bucket access to cloudFront*.

*Origin settings*: Use recommended origin settings.

5.	Click ***Next*** to go to the next page.

6.	*Web Application Firewall (WAF)*: Select ***Do not enable security protection***.

*WAF is not available on Free Tier, besides I don’t really need that level of security for a static website like this.*

7.	***Next*** to move to the next step.

8.	Click ***Create distribution***

 On *CloudFront* > *Distributions page*, find your newly created distribution on the distributions list. Review the status to see it is enabled. If it isn’t enabled, wait a few minutes and refresh the list.

![CloudFront Distributions][009]

Wow! Congratulations! We have just created a cloudfront distribution for our website. Cloudfront also creates a new URL we can use to access our new website. Review the page *CloudFront* > *distributions*.

9.	In *CloudFront* > *Distributions*, click on the ID of your new distribution.

10.	On the *General* tab of the distribution, under *Details* find the *Distribution domain name*. This is our new URL to access our website through CloudFront. It should look like *'random text'.cloudfront.net*

11.	Copy the distribution domain name, open a new browser tab and paste it in the address bar.

![CloudFront access denied][010]

Ouch! Another error. It says *Access denied*. But we already gave Amzon CloudFront access to our bucket!

No worries though, now we just need to tell CloudFront which document exactly to index. That should do it.

### Specify default root object to CloudFront
1.	In *General* tab of our distribution, under *Settings* click on the ***Edit*** button.
2.	Under *Default root object*, type in *index.html*. 
3.	Click ***Save changes***

![CloudFront settings][011]

4.	Now let's head back to the browser tab with our CDN URL and refresh the browser.

![CDN now working][012]

Yo! We did it! Our website is up and ready to go.

## Testing
We know our CloudFront setup is working and we can access our website through the distribution domain name that CloudFront generate dfor us.

We can check our S3 bucket website endpoint to see if it is publicly accessible too. It is not.

## Conclusion
We have successfully hosted our static website on Amazon S3 and configured our website contents to be served strictly through Amazon cloudfront as the CDN. WE now have a fast,  highly-scalable, secure and high performance website.


This project can be extended by incorporating a customized domain name from Amazon Route 53 and request a public SSL certificate from Amazon Certificate Manager. We can also setup a CI/CD pipeline through Github Actions to automate code deployments straight from github.





