sphix-travisci-s3-example
=========================

This is an example demonstrating continuous deployment of docs to S3 using
Travis-CI.

The idea is that after Travis-CI runs your tests (I assume you're using
Travis-CI already), if the tests pass we'll use the Travis-CI virtual machines
to build the documentation and push to Amazon S3. This way, your documentation
stays current as you develop.

The example docs for this project are deployed at: http://sphnix-travisci-s3-example.s3-website-us-west-1.amazonaws.com/
Note that for a real project, you can set up a [custom domain](http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html)

Here are some instructions on replicating this process for your own github
project.

1. If you don't already have one, create an Amazon AWS account. From the
   security credentials page, create an access key (Access Key ID and Secret
   Access Key), and set them as the `AWS_ACCESS_KEY_ID` and `AWS_ACCESS_KEY_ID`
   environment variables in your shell session.
2. Install the travis gem. Note that this requires a working ruby installation.
    ```
    gem install travis
    ```
2. Encrypt the `AWS_ACCESS_KEY_ID` and `AWS_ACCESS_KEY_ID` environment variables
   by running
   ```
   $ travis encrypt AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
   $ travis encrypt AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
   ```
   from the shell, and adding the resulting encrypted string into your `.travis.yml`
   file. Some more details on Travis-CI encrypted environment variables can be
   found here: http://docs.travis-ci.com/user/encryption-keys/
3. Create an Amazon S3 bucket, and set the name of the bucket in your .travis.yml`
   file (see the one in this project for an example)
4. Configure your bucket to allow public access by changing the bucket policy
   to something like. This is under the permissions tab.
```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "AddPerm",
			"Effect": "Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::your-bucket-name-here/*"
		}
	]
}
```
5. In the S3 web interface, also enable static website hosting. There is some
   more information about static website hosting at
   http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html.
