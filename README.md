# fluentd

This is a preliminary fluentd chart to include configs for s3 bucket. not production ready. 
```
helm install ./fluentd/ --name fluentd --set awsKeyId="$AWS_ACCESS_KEY_ID",awsSecKey="$AWS_SECRET_ACCESS_KEY",s3Bucket=your-bucket,s3Region=your-region
```
This will collect logs forwarded from [fluent-bit](https://github.com/samsung-cnct/chart-fluent-bit) and send them to an s3 bucket as well as on to [elasticsearch](https://github.com/samsung-cnct/chart-elasticsearch).  