# fluentd


this is a preliminary fluentd chart to include configs for s3 bucket. not production ready. 
```
helm install ./fluentd/ --name fluentd --set awsKeyId="$AWS_ACCESS_KEY_ID",awsSecKey="$AWS_SECRET_ACCESS_KEY",s3Bucket=your-bucket,s3Region=your-region
```
