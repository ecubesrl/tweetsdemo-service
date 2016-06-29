#### An Ambari service to deploy a real-word data ingestion from Twitter to Solr with NiFi
This tweets demo service shows real time tweets on a dashboard. It requires HDFS, Kafka, NiFi and Solr already installed and started on the Hortonworks Sandbox. You also need a Twitter account and a Twitter application token (follow the instructions below to create one).

##### Credits:
  - Davide Isoardi: data modelling, NiFi workflows and Solr collections 
  - Davide Vergari: demo architecture and Ambari Service

Future improvments:
  - Giuseppe Maldarizzi: Storm topologies and custom real time visualization
  - Marco Gaido: Spark machine learning and Python improvments to Ambari Service

Prerequisites:
  - HDP Sandbox 2.4
  - The service requires that HDFS, Kafka and Solr are started on the Sandbox.
  - Apache NiFi must be installed and started; use [this](https://github.com/abajwa-hw/ambari-nifi-service) guide to install Apache NiFi.
  - You need admin rights to Ambari. To enable admin access on Ambari follow the instructions below:
    - Login to the Sandbox with SSH or via [Shell in a box](http://sandbox.hortonworks.com:4200)
    - Run the following command:
```
ambari-admin-password-reset
```

Limitations:
  - Currently, this demo runs only on a HDP Sandbox, do not use it in production clusters. 
  - It does not support Ambari/HDP upgrade process and will cause upgrade problems if not removed prior to upgrade.



##### Setup steps

- Download HDP 2.4 Sandbox Virtual Machine Image from [Hortonworks website](http://hortonworks.com/products/hortonworks-sandbox/)
- Import Hortonworks Sandbox into your virtualization engine.
- Start the VM.
- If using VirtualBox, add port forwarding to port 9090 to allow your browser access NiFi Web UI if not already configured. Rembember to add also the following line to your hosts file:
```
127.0.0.1 sandbox.hortonworks.com localhost
``` 
- If using VMWare find the IP address of the VM and add an entry into your machines hosts file. For example:
```
192.168.191.241 sandbox.hortonworks.com sandbox 
```
- Install and start Apache NiFi following [these](https://github.com/abajwa-hw/ambari-nifi-service) instructions (if not already installed).
- **Make sure Kafka is running and out of maintenance mode**. 
- Start Solr by connecting via SSH to the VM and run the following command:
```
/root/start_solr.sh
```
- Before proceeding, you may want to wait a few minutes to ensure they stay up reliably or the demo setup may fail. If they do not, you may need to increase the memory/cpus allocated to the VM.
- Deploy the Tweets Demo Ambari service:
```
VERSION=`hdp-select status hadoop-client | sed 's/hadoop-client - \([0-9]\.[0-9]\).*/\1/'`
git clone https://github.com/ecubesrl/tweetsdemo-service.git /var/lib/ambari-server/resources/stacks/HDP/$VERSION/services/TWEET
ambari-server restart
```
- Then you can click on 'Add Service' from the 'Actions' dropdown menu in the bottom left of the Ambari dashboard:

On bottom left -> Actions -> Add service -> 'Tweets Demo' -> Next -> Next -> Configure service -> Fill required fields -> Next -> Deploy

- Remember to fill the required fields while configuring the demo. You can create your own Twitter access token by following [these](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) instructions. You can change also following your needs filter_terms fields to grab tweets.


- On successful deployment you will see the Tweets Demo service as part of Ambari stack and will be able to start/stop the service from here:

- Connect to the Banana interface to see geo-localized tweets http://sandbox.hortonworks.com:8983/solr/banana/index.html#/dashboard
- Connect to the Kiwi interface to see all grabbed tweets http://sandbox.hortonworks.com:8983/solr/kiwi/index.html#/dashboard

- Connect to the NiFi interface to take a look at twitter_dashboard Process Group on the NiFi Web UI at http://sandbox.hortonworks.com:9090/nifi/
