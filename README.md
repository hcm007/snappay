# SnapPay project
:blush:	 :blush:	 :blush:	 :blush:	

## Question_1 You are asked to build an Elasticsearch cluster and Kibana server in cloud to collect logs

### How to provision the cluster in 30 minutes

#### Prerequistites
```
git clone git@github.com:pires/kubernetes-elasticsearch-cluster.git
git clone git@github.com:fluent/fluentd-kubernetes-daemonset.git
```

you need to configure the aws command credentials

```
cd eks_terrafrom
terraform plan
terraform apply

```
After provision the eks cluster, we can use kubectl in master nodes

###  How to scale the cluster up or down

```
cd kubernetes-elasticsearch-cluster
kubectl scale --replicas=$NUMBER -f es-master.yaml
```

### How to prevent unauthorized access to the cluster
1. we can use vpn to access to our cluster
2. we can use TLS to ecrpty the elasticsearch 
3. create username or password for the fluentd configure

### How to ship logs from application to the elasticsearch cluster
we can use any agents to ship the logs to elasticsearch cluster, in this case, I choose fluentd to ship the logs, because I can customize the logs form
```
cd fluentd-kubernetes-daemonset
kubectl create -f fluentd-daemonset-elasticsearch-rbac.yaml
```
you can custimize the logs based on the fluentd configuration
 
deploy the kibana

```
cd kubernetes-elasticsearch-cluster
kubectl create -f kibana-cm.yaml
kubectl create -f kibana.yaml
kubectl create -f kibana-svc.yaml
```

### How to monitor the performance of the cluster
1. we can use prometheus to monitor the cluster
2. we can use third party tool like datadog, use the datadog agent to send the matrix to datadog
3. or just use the aws native monitoring tool cloudwatch 

### What about if the applications are running in multiple regions/locations
1. if we still use the AWS so we can use vpc peer connection which means two vpc can communicate with each other
2. if we use the different cloud provider or we host application, we can deploy fluentd agent to the different regions and send logs to elasticsearch cluster,
3. if we use serverless like lambda or cloud funcrion, we need write some code in our application to push logs to elasticsearch API.
4. if we have hundreds of application with diff techs, like docker, host, serverless, then I choose Kafka to distribute the logs, and we can make sure the logs can be transfer in real time

Because kafka is a big topic, I don't have time to give you more detail


## Question_2 host static website or content

### Prerequistites
The users need to configure the aws command credentials
Or repository is not limited to aws cloud, we can add gcp, azure or 阿里爸爸 :blush: with terraform

### the code to create all necessary infrasture for the website
the following commands to create the s3 bucket and CDN. if you want to use loadbalancer, you can ask the contributor to work for u and he will add more
 
```
cd s3-webfiles-with-cloudfront
terraform plan
terrafrom apply
```

### the pipeline to deploy files from the git repo to the website
1. For Source code management, you can use gitlab, bitbucket or github
2. for CI/CD pipeline tool, can use jenkins(I like it), bamboom 
3. Create jenkins job which can be triggered by gitlab push or github push

### - be able to rollback to any version deployed in the last 3 month
1. when you push the code to gitlab or github please create tag to release
2. Then jenkins job can be passed the variable which is tag or version to do rollback
3. or if you can use the commit number to ralloback

### - be able to support HTTPS
1. create certificate for *.snappay.ca
2. create DNS records in godaddy or route53 like cdn.aws.net cname www.snappay.ca
2. assgin the certicate to the cdn or loadbalancer

# :sunglasses: 我在原地等你的回复 :sunglasses: #






