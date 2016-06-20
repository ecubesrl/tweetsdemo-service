#### An Ambari service to deploy a real-word data ingestion from Twitter to Solr with NiFi
This tweets demo service show real time tweets on a dashbord. It requires Kafka, NiFi and Solr already installed and started on the Hortonworks Sandbox

Pre-reqs:
  - HDP Sandbox 2.4.x
  - The service requires that Kafka and Solr are started on the sandbox.
  - Apache NiFi must be install and started; use [this](https://github.com/abajwa-hw/ambari-nifi-service) link to install Apache NiFi.
  - You need admin rights to the sandbox. To enable admin access on the sandbox follow the instructions below:
    - Login to the sandbox
    - Run the following command:
```
ambari-admin-password-reset
```

Limitations:
  - Currently, this demo runs only on a Sandbox, do not use it in production cluster. 
  - It does not support Ambari/HDP upgrade process and will cause upgrade problems if not removed prior to upgrade.

##### Setup steps

- Download HDP 2.4 sandbox VM image (HDP_2.4_virtualbox_v3) from [Hortonworks website](http://hortonworks.com/products/hortonworks-sandbox/)
- Import Hortonworks Sandbox into your virtualization engine.
- Start the VM.
- If using VirtualBox, add port forwarding to port 9090 to allow your browser access NiFi Web UI.
- Install and start Apache NiFi following [these](https://github.com/abajwa-hw/ambari-nifi-service) instructions.
- **Make sure Kafka is running and out of maintenance mode**. 
- Start Solr by connecting via SSH and run the following command:
```
/root/start_solr.sh
```
- Before proceeding, you may want to wait a few minutes to ensure they stay up reliably or the demo setup may fail. If they do not, you may need to increase the memory/cpus allocated to the VM.
- Deploy the tweetsdemo-service
```
VERSION=`hdp-select status hadoop-client | sed 's/hadoop-client - \([0-9]\.[0-9]\).*/\1/'`
git clone https://github.com/dvergari/tweetsdemo-service.git /var/lib/ambari-server/resources/stacks/HDP/$VERSION/services/TWEET
ambari-server restart
```
- Then you can click on 'Add Service' from the 'Actions' dropdown menu in the bottom left of the Ambari dashboard:

On bottom left -> Actions -> Add service -> 'Tweets demo' -> Next -> Next -> Configure service -> Fill required fields -> Next -> Deploy

- Remember to fill the required fields while configuring the demo. You can create your own Twitter access token by following [these](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) instructions.


- On successful deployment you will see the TWEETDEMO service as part of Ambari stack and will be able to start/stop the service from here:

- Connect to the Banana interface to see the tweets http://localhost:8983/solr/kiwi/index.html#/dashboard

- Also, you will see a new Process group on the NiFi Web UI with all components started

