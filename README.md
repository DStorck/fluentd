# fluentd

This is a preliminary fluentd chart to include configs for s3 bucket. not production ready. 


Before installing chart ** need to load tls secrets into cluster 
```
kubectl create secret generic fluentd-tls \
  --from-file=ca.crt.pem=./certs/ca.crt.pem \
  --from-file=server.crt.pem=./certs/server.crt.pem \
  --from-file=server.key.pem=./private/server.key.pem
  ```



```
helm install ./fluentd/ --name fluentd --set awsKeyId="$AWS_ACCESS_KEY_ID",awsSecKey="$AWS_SECRET_ACCESS_KEY",s3Bucket=your-bucket,s3Region=your-region,fluentdPrivateKeyPassphrase=your-ssl-passphrase
```
This will collect logs forwarded from [fluent-bit](https://github.com/samsung-cnct/chart-fluent-bit) and send them to an s3 bucket as well as on to [elasticsearch](https://github.com/samsung-cnct/chart-elasticsearch).  