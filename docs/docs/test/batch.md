# Testing the most basic job

From the gateway node

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{
   "applicationName": "ubuntu",
   "version": "latest",
   "type": "batch",
   "entryPoint": "sleep 10",
   "instances": 1,
   "cpu": 1,
   "memory": 1024,
   "disk": 10000,
   "networkMbps": 128
 }' 'http://localhost:7001/api/v2/jobs'
```