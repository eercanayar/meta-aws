### Setup Cloud services
```
# aws cloudformation create-stack --stack-name gg-demo --template-body file://lambda.yml  --capabilities CAPABILITY_IAM
```

### Get services info

```
# aws cloudformation describe-stacks  --stack-name gg-demo | jq '.Stacks[0].Outputs'
[
  {
    "OutputKey": "BucketName",
    "OutputValue": "gg-demo-s3bucketforwebsitecontent-abcde123456"
  },
  {
    "OutputKey": "IdentityPoolRoleArn",
    "OutputValue": "arn:aws:iam::157885332704:role/gg-demo-CognitoUnAuthorizedRole-1VN3A8N5RR02I"
  },
  {
    "OutputKey": "RestAPI",
    "OutputValue": "https://k9i1vpdqe3.execute-api.us-east-2.amazonaws.com/v1/things"
  },
  {
    "OutputKey": "WebSite",
    "OutputValue": "d30j59igkhuwa2.cloudfront.net"
  },
  {
    "OutputKey": "IdentityPoolID",
    "OutputValue": "us-east-2:efbc2ea9-4491-42a0-9fc8-b638757eb8bd"
  }
]
```

### DashBoard Build Steps
- [ ] Set `window.REGION` in `src/Map.js` line 23
- [ ] Update CognitoIdentityCredentials configuration in `src/Map.js` line 121, 122
- [ ] Update googlemap `bootstrapURLKeys` or delete it for dev environment in `Map.js` line 409
- [ ] Update RestAPI endpoint. (Map.js line 342)
- [ ] npm install
- [ ] npm run build

### Upload dashboard to s3 bucket

Download `s3deploy` from: https://github.com/bep/s3deploy/releases/tag/v2.7.0

Then run the following by replacing your bucket, region and CloudFront distribution ID:

```
$ s3deploy -bucket gg-demo-s3bucketforwebsitecontent-abcde123456 -region eu-west-1 -config s3deploy.yml -acl private -source build -distribution-id ABCDE123456
 
```

### Update device Latitude and Longitude:
```
# aws iot update-thing --thing-name neawsdemo_GreengrassCore --attribute-payload '{"attributes":{"lat":"39.906902","lng":"116.463604"}}'
```

### Delete cloud services
```
# aws s3 rm  --recursive s3://gg-demo-s3bucketforwebsitecontent-abcde123456
# aws cloudformation delete-stack  --stack-name gg-demo
```
