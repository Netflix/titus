To send requests, curl jobs to the gateway node

# Testing the most basic job
```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
   "applicationName": "ubuntu",
   "version": "latest",
   "type": "batch",
   "entryPoint": "sleep 10",
   "instances": 1,
   "cpu": 1,
   "memory": 1024,
   "disk": 1000,
   "networkMbps": 128
 }' 'http://localhost:7001/api/v2/jobs'
```

# Testing the most basic job
```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
   "applicationName": "ubuntu",
   "version": "latest",
   "type": "batch",
   "entryPoint": "sleep 10",
   "instances": 1,
   "cpu": 1,
   "memory": 1024,
   "disk": 1000,
   "networkMbps": 128,
   "iamProfile": "arn:aws:iam::ACCOUNTID:role/IAMPROFILENAME" 
 }' 'http://localhost:7001/api/v2/jobs'
```

# Testing the metadataservice without executor or VPC driver
There is a a testing flag you can add to the metadata-service run command `export ALLOW_REMOTE_IP_OVERRIDE=true`
which will allow you to override the connection IP used to lookup the container doing a metadata request. If you
start two containers with the below labels:

```
sudo docker run -d \
  --label TITUS_TASK_INSTANCE_ID=abf580cb-740b-4ab0-a46a-ebb5d590dbfe \
  --label ec2.iam.role=arn:aws:iam::ACCOUNTID:role/titusappwiths3InstanceProfile \
  --label titus.net.ipv4=1.1.1.1 \
  --label titus.task_id=Titus-15125145-worker-0-52 \
  --label titus.vpc.ipv4=1.1.1.1 \
  ubuntu:latest sleep 1000000

sudo docker run -d \
  --label TITUS_TASK_INSTANCE_ID=abf580cb-740b-4ab0-a46a-ebb5d590dbfe \
  --label ec2.iam.role=arn:aws:iam::ACCOUNTID:role/titusappnos3InstanceProfile \
  --label titus.net.ipv4=1.1.1.2 \
  --label titus.task_id=Titus-15125145-worker-0-52 \
  --label titus.vpc.ipv4=1.1.1.2 \
  ubuntu:latest sleep 1000000
```

You can test the IAM support by running curl with a special header:
```
curl --header "remote-ip:1.1.1.1" http://10.11.10.11:9999/latest/meta-data/iam/security-credentials/titusappwiths3InstanceProfile

curl --header "remote-ip:1.1.1.2" http://10.11.10.11:9999/latest/meta-data/iam/security-credentials/titusappnos3InstanceProfile
```

# Debugging Mesos
- IPOFMESOS:5050
- IPOFMESOS:5050/slaves
