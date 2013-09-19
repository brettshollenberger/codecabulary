# Amazon Web Services (AWS)

80-90% of end user response time consists of downloading page components--the images, stylesheets, scripts, etc--that make up your page. One of the best ways to improve page performance is to distribute those resources across multiple edge locations so that a user can load the resource from the location closest to them. Amazon's Content Delivery Network (CloudFront) works by pointing to a canonical copy of an individual resource, and then distributing that resource to its other locations and serving individual requests from those copies. 

#### S3

Amazon's Simple Storage Service (S3) is just that--a key/value pair service that allows you to interact with a canonical copy of your data via a RESTful API (or a web interface). 

Namespaces in S3 are called buckets. They're the fundamental container for data storage. Objects can be retrieved from by combining the namespace, key, and (optionally) version:

	my-namespace.s3.amazonaws.com/images/image.jpg
	
In the above, `my-namespace` is the bucket, and `images/images.jpg` is the key used to refer to the image value. 

#### S3 Primarily Uses Eventual Consistency

Most changes to data stored on S3 will propagate eventually--usually within a few seconds. Many regions on S3 also provide read-after-write consistency, so new files will propagate immediately, but updated and deleted files may still take time to propagate.

#### Authenticating S3 Requests

S3 provides you, the developer, with an Access Key and a Secret Access Key. When you make requests to the S3 API, you include in the meta information your Access Key, which Amazon uses to look up your Secret Access Key in its database. You'll also include your "signature" in the metadata--an HMAC-SHA1 encrypted copy of the request using your Secret Access Key as the cryptographic key. Amazon then uses its copy of your cryptographic key to decode the message, and provided the message matches the signature, your requests are authenticated.

S3 also allows unencrypted (anonymous) requests to S3 by enabling public access to certain resources. By default, all resources are available only to you, so you'll need to remember to provide public access to these resources in order to hook up S3 with Cloudfront.

#### S3 + CloudFront

CloudFront is the CDN that we'll use to improve performance, hooking up to the S3 canonical copies of our resources. CloudFront will cache copies of content close to viewers, giving us access to scalable, low-latency, high-transfer-rate distributions for dirt cheap.

Instead of the requested resources having to travel along interconnected networks to fulfill our users' requests, CloudFront will route end users to the nearest edge location automatically.. 

#### Creating a CloudFront Distribution

Hooking up to the S3 service is one of the default options for CloudFront (though we could also host our canonical copies on our own endpoints). Once we hook up and the data propagates, we can begin using CloudFront as our API for distribution of static content, greatly improving the quality of our service, particularly for image-heavy or downloadable-content heavy sites like Instagram or YouTube. 