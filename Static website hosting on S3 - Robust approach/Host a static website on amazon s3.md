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


# Host a static website on amazon s3 and configure dns with cloudfront

## About this project;
In this project, I setup a static website on AWS. Amzon s3 is an object storage service offered by amazon to provide highly scalable, durable and available platform for storing and retrieving any amount of data from anywhere in the world, so it is just the perfect tool for hosting our static website. The website contents will be html, css, javascript and image files from my first ever portfolio page created while I was just learning how to code. 
This project also involves use of amazon cloudfront as a content delivery network to speed up serve our website content by caching content closer to global viewers, reducing redundancy and improving our website’s performance.
At the end of this project, we will have a static website that is highly scalable, fast and secure.

## Technologies used in this project
Simple storage servive – for hosting our web contents.
Cloudfront – a content delivery network by aws

### Overview of steps
1.	Write and prepare our website files
2.	Create an s3 bucket
3.	Upload website files
4.	Configure static web hosting
5.	Create cloudfront distribustion
6.	Testing

… and so we begin

### Write and prepare our website files
Amazon s3 is suitable for hosting static websites, which consist of contents like html, css and javascript. It does not natively support server-side processing or databases required for dynamic websites unless in conjunction with other aws services.
I have an already prepared folder of files from an old project, so I’ll be using that here.

### create an s3 bucket
It took me about 2 minutes to create this bucket, which goes to show how convenient and efficient cloud solutions can be.
I picked the northern California region to provision my s3 bucket because it is the closest to me, since I live in California. When working with aws services, it is important to provision your services in regions that that closest to your users.
1.	Go to the amazon s3 console and click ‘create bucket’
2.	Enter a unique bucket name in the field provided for ‘bucket name’
S3 bucket names are globally unique1 because all amazon s3 bucket names share a single, global namespace across all aws accounts and regions. Our bucket names must be as unique as possible
3.	For object ownership, we will leave it as ACLs disabled.

![Create bucket][001]



Access control lists or Acls are an aws legacy way of controlling ownership of objects written to our bucket. For the security of our web content, we will leave that as disable. Aws recommends this too.
4.	For ‘public access settings for this bucket’, I checked the ‘block all public access’ button.
Ordinarily, to make our website contents accessible through the internet, we would be required to not check this box, but we want to create a highly secure website. So, I’m going to leave this box checked and later grant access to only cloudfront using something called bucket policies.


5.	Enable bucket versioning if you want s3 to store multiple versions of objects after every update. This will help with easilily recovering from application failures and unintended user actions. I will enable this.

![Create bucket][002]

6.	We will leave the rest of the settings ‘default encryption’ and ‘advanced settins’ as is and just scroll to the button and create bucket.

![View bucket][003]
And there you go, we have just created our s3 bucket, and can see our new bucket if we go to the s3 console and on the left pane click on general purpose buckets.


### Upload website files to our s3
It is now time to upload our prepared web content file from step to our s3 bucket. Note that the index page of our website must be name index.html. a website’s index page is the html document to the website’s home page.
1.	From our s3 general purpose buckets list, click on the bucket name, i.e the link under the names column of our list. This will take you to the objects page of our bucket.
2.	Click on the ‘upload’ button, and on the upload page, you can drag and drop all the files from our local computer to the field provided.

![upload content][004]

3.	Click the upload button and continue to the next step.
You should get an upload succeeded alert. Review the list of your uploaded files and click the close button to go back to our objects list page.

![upload successful][005]

Also review the tabs above our objects list. The tabs are objects, properties, permissions, metrics etc.

### Configure static web hosting
Website hosting is what makes our website available on the internet for anyone who has the domain address to be able to view our files. Without hosting, our website is just another bunch of files stored in the cloud and no one can see them.
To enable website hosting on our s3 bucket…
1.	While inside our bucket, click on the properties tab.
2.	Scroll all the way down to the static website hosting section and click on the edit button.
3.	Under static website hosting, select the radio button for enable.
4.	In the field provided for index document, type in index.html.

![Enable static web hosting][006]

5.	Click on save changes button.
You have successfully edited your static website hosting. AWS will generate bucket website endpoint. 
The bucket website endpoint is the URL created by AWS through which our static website can be accessed on the internet.

![Bucket website endpoint][007]

6.	Copy the bucket website endpoint (your new website URL) and paste in a new tab of your web browser.
at this point, we see a 403 forbidden telling us access to the s3 contents is denied. 

![403 Forbidden][008]

But no worries. We turned off public access to our bucket, so no one can view our files directly from s3. We want a secure and robust solution, so will be leave public access turned off and instead allow cloudfront the permission to serve our web contents instead.

So, lets head over to cloudfront and implement this ….

## Create a cloudfront distribution
1.	Go to cloudfront console -> Click ‘create distribution’
2.	Enter a name for your distribution, and select single website or app in the distribution type section.
3.	Click Next to move to the next step.
4.	In the origin settings;
Origin domain; select Amazon S3
Origin path: leave blank
S3 origin: Click Browse S3 and select the s3 bucket for your site.
Here, AWS may prompt you to use website endpoint. Do not use this.
Settings: Select Allow private S3 bucket access to cloudFront.
Origin settings: Use recommended origin settings.
5.	Click Next to go to the next page.
6.	Web Application Firewall (WAF): Select Do not enable security protection.
WAF will cost us money, and we don’t really need it for a static website like this.
7.	Next to go to the next step
8.	Click Create distribution
 On cloudfront > Distributions page, find your newly created distribution on the distributions list. Review the status to see it is enabled. If it isn’t enabled, wait a few minutes and refresh the list.

![CloudFront Distributions][009]

Wow! Congratulations! We have just created a cloudfront distribution for our website. Cloudfront also creates a new URL we can use to access our new website. Review the page Cloudfront > distributions.
9.	In cloudfront > Distributions, click on the ID of our new distribution.
10.	On the general tab of the distribution, under Details find the distribution domain name. This is our new URL to access our website through cloudfront. It should look like <random text>.cloudfront.net.
11.	Copy the distribution domain name, open a new browser tab and paste it in the address bar.

![CloudFront access denied][010]

Ouch! Another error. It says access denied. 
But we already gave cloudfront access to our bucket!
No worries though, now we just need to tell cloudfront which document exactly to index. 

### Specify default root object to cloudfront
1.	In General tab of our distribution, under settings click on the edit button.
2.	Under default root object, type in index.html. 
3.	Click save changes

![CloudFront settings][011]

4.	How head back to the browser tab with our cdn url and refresh the browser.

![CDN now working][012]
Yo! Our website is up and ready to go.

## Testing
We know our cloudfront cdn is working and uses can access our website through the cdn distribution we created.
We can check our s3 bucket website endpoint to see if it is publicly accessible too.

## Conclusion
We have successfully hosted our static website on S3 and configured our website contents to be served strictly through cloudfront as a cdn. WE now have have a fast,  highly scalable, robust, secure and high performance website.
This project can be extended by incorporating a customized domain name from Amazon route 53 and request a public SSL certificate from Amazon certificate manager. We can also setup a ci/cd pipeline through github actions to automate code deployments straight from github.





