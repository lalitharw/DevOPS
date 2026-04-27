# Create s3 and Upload Image to Laravel

### While Creating s3
```
While Creating s3 disable all public block option. We have to uncheck that so that we can access file publicly.
```

### Create a Bucket Policy
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadAccess",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::laravel-loadbalancer-files/*"
    }
  ]
}
```

### Create IAM User and Give Permissions
```
Create User and click on create access key. It will generate access key and secret key which should be used to upload image to s3 bucket. In Permissions Section search for "AmazonS3FullAccess".
```

### Reference
https://www.honeybadger.io/blog/laravel-s3/
