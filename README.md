# fluentd helm chart for kubernetes 

This is a preliminary fluentd chart to be used with [fluent-bit](https://github.com/samsung-cnct/chart-fluent-bit) on a kubernetes cluster. Fluent-bit will be deployed as a daemonset to collect all logs and will then push them to fluentd using the forward protocol, secured by TLS. FLuentd will pass on logs to [elasticsearch](https://github.com/samsung-cnct/chart-elasticsearch), secured by [xpack](https://www.elastic.co/guide/en/elastic-stack-overview/current/elasticsearch-security.html), and to [s3](https://aws.amazon.com/s3/) for long term archival. 


### Log Archival using s3 
Fluentd has a plugin to push logs to s3. This plugin must in included in the values file under the `plugins` section to be installed.  You must first go into s3 and create the bucket and path that you wish to push to. Then create either use or create an IAM role that has s3 access. 

Several pieces of information must be passed in at chart install for this to work properly, as the plugin needs access to your aws credentials, and needs to know which region and bucket you would like to push the logs to. 
Make sure the following envs are set: `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`

You will also need to provide s3 bucket name and region at chart installation. 
For example: `s3Bucket=myLogs` and `s3Region=us-west-2`

### Secure forward of logs using TLS 
TLS information must be passed in as a secret to the cluster before this chart can be deployed. 
```
kubectl create secret generic fluentd-tls \
  --from-file=ca.crt.pem=./certs/ca.crt.pem \
  --from-file=server.crt.pem=./certs/server.crt.pem \
  --from-file=server.key.pem=./private/server.key.pem
  ```
  
Ensure that client certs are passed into secrets as well and used by fluent-bit. 

## To install fluentd from local repository 
```
helm install ./fluentd/ --name fluentd --namespace=your-namespace --set awsKeyId="$AWS_ACCESS_KEY_ID",awsSecKey="$AWS_SECRET_ACCESS_KEY",s3Bucket=your-bucket,s3Region=your-region,fluentdPrivateKeyPassphrase=your-ssl-passphrase
``` 

Resources: 
- [fluentd s3 documentation](https://docs.fluentd.org/v1.0/articles/out_s3)
- [s3 plugin source code](https://github.com/fluent/fluent-plugin-s3)
- [Article about kubernetes and secure forward](https://banzaicloud.com/blog/k8s-logging-tls/) 
