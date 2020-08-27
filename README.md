# sample.edge-rolling-average

This sample contains a simple Python application that you can quickly build for the edge, deploy to the edge, and observe the application as it runs on the edge.  The application computes a rolling average of a value for a group of sensors.  For the purposes of this sample, all the sensor data is generated.

## Before you begin

1. [Install IBM Cloud Pak for Data (CP4D) 3.0.1](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/install/install.html)
    - Gather the following information for use in this scenario
        - **_web client URL_**: This is the URL used to access the IBM Cloud Pak for Data environment in your browser. It should be of the form: https://HOST:PORT (e.g., https://123.45.67.89:12345).
        - **_credentials_**
        : These are the credentials (username and password) used to log in to the IBM Cloud Pak for Data environment in your browser. 
        - **_version_**: You can find the version number in the About section after logging in to the IBM Cloud Pak for Data environment in your browser.
2. [Install IBM Edge Analytics beta service on CP4D](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/install.html) and [setup edge systems](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/admin.html)
    - Gather the credentials (root password) for Edge nodes for use in this sample
3. [Install IBM Streams 5.4.0 service on CP4D](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/streams/install-intro.html)
4. [Install Watson Studio service on CP4D](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/wsj/install/install-ws.html)
5. [Provision a Streams instance](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/streams/provision.html#provision)

6. If IBM Edge Application Manager will be used to managed edge application lifecycles
    - [Install IBM Edge Application Manager 4.1](https://www.ibm.com/support/knowledgecenter/SSFKVV_4.1/hub/hub.html)
    
        - Gather the following information for use in this sample
            - API key for IEAM access  ( _eam-api-key_ )
    - Reference Openshift administrator information
    
        - Gather Openshift cluster url & credentials for use in this sample
            - _openshift-cluster-url:port_
            - _default-route-to-openshift-image-registry_
            - _openshift-token-for-cpd-admin-sa_

## Instructions

#### 1. Copy this notebook into an IBM Cloud Pak for Data project
- Copy the GitHub URL of the Edge-RollingAverage.ipynb file.
  - https://github.com/IBMStreams/sample.edge-rolling-average/blob/main/Edge-RollingAverage.ipynb
- Create a new, empty project or open an existing project in IBM Cloud Pak for Data.
- From your project, click "Add to project" and select "Notebook".
- Select the "From URL" tab, enter a name for the notebook (e.g. Edge-RollingAverage) and paste the URL in the "Notebook URL" field.
- Click "Create notebook".

#### 2. Edit the notebook to specify the name of your Streams instance
- Find the line of code with `streams_instance_name` in the last cell in the notebook. This line has a comment requesting its change.
- Replace the `sample-streams` value with the name of your provisioned Streams instance.
- Save your notebook.

#### 3. Run the notebook
- Run each cell in the notebook.
- The last cell in the notebook will submit a request to your Streams instance to build the application for the edge.  It will take a little while to complete.  While it is running, a progress bar entitled "Building Bundle" will appear momentarily, followed by a "Building edge image" that will display its progress.

  - The build will create a Docker image that can run on an edge system.
  - After a few minutes you should see a message indicating that the build was successful.
  - An additional message will indicate the full path to the Docker image for the application. 
```
Application image built successfully.
    Image:    image-registry.openshift-image-registry.svc:5000/ivan34/edge-sensorrollingaverage:streamsx
```
  - The image name that this sample produces is:  "edge-sensorrollingaverage:streamsx".  If you are using EAM for deployment, take note of the imagePrefix, the name segment just prior to this name.  In this example, the imagePrefix is _ivan34_.  It will be needed in a later step.

### All of the instructions to this point are independent of the choice of deployment methods.  However, the instructions will now diverge.  To keep the instructions simpler, there is a separate set of instructions for each deployment method.

### Deploying an Edge application using CP4D 

#### CP4D.4. Package and deploy the application to one or more edge systems 
    
- From CP4D Console, perform these steps. For more information, see [Packaging using Cloud Pak for Data](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-register-by-cpd.html) topic. 
    1. Select CPD Console > Navigation Menu > Analyze > Edge Analytics > Analytics apps
    1. Click 'Add Application packages' and fill in these values
        | Field | Value |
        | ----- | ----- |
        | Name | Rolling Average Sample | 
        | Version | 1.0 |
        | Image reference | edge-sensorrollingaverage:streamsx |
    1. Save
    
     
#### CP4D.5. Deploy application package to an Edge node 
From CP4D Console perform these steps. For more information, see [Deploying using Cloud Pak for Data](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-deploy-cpd.html) topic.  
1. Continuing from the 'Analytics apps' panel
    - CPD Console > Navigation Menu > Analyze > Edge Analytics > Analytics apps
1. Go to end of the row with "Rolling Average Sample" and click on three dots to open list of options, and select 'Deploy to edge'
    1. When the list of remote systems is displayed, check the box next to the remote system you want to deploy to.
    1. Select 'Deploy' option.
1. To verify that the app was deployed successfully, select the "Rolling Average Sample"
    1. Verify that there is an application instance for the deployment to your chosen system.  If the 'Last known status' shows a symbol with fly-over text of 'Provision in progress', that status may need to be refreshed.  This can be done by selecting that row, continue over to far right of that row, and select "Open list of options" (represented by three vertical dots).  Selecting 'Refresh status' should update the 'Last known status' to a symbol with flyover text of "Running".

#### CP4D.6.  Observe the application running on the edge
From CP4D Console, perform these steps.  See [Monitoring edge systems and applications with Edge Analytics](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-monitor.html) for detailed instructions on how to view status, logs, metrics, etc. for edge applications.
1. Continuing from the 'Rolling Average Sample' panel
    - CPD console > Navigation Menu > Analyze > Edge Analytics > Analytics apps > rolling average sample
1. Go to row for the application instance for the edge node that you would like to see log for, and select three dots at clear right part of row to see the list of options.
    1. Select 'Download logs'.
1. Unzip downloaded log package.
1. Open up rolling-average-sample-xxxx.log file
    - This file contains a variety of statements.  The standard println output will be in this log, as well as the output from the trace statements.

```2020-08-27T13:15:23.047502063-07:00 stdout F {'average': 1729.796946015279, 'sensor_id': 'sensor_1', 'period_end': 'Thu Aug 27 20:15:23 2020'}
2020-08-27T13:15:23.047715310-07:00 stdout F {'average': 1321.2858402449626, 'sensor_id': 'sensor_10', 'period_end': 'Thu Aug 27 20:15:23 2020'}
2020-08-27T13:15:23.047765359-07:00 stdout F {'average': 411.0143925640869, 'sensor_id': 'sensor_3', 'period_end': 'Thu Aug 27 20:15:23 2020'}
2020-08-27T13:15:23.047806104-07:00 stdout F {'average': 1788.7616988434274, 'sensor_id': 'sensor_4', 'period_end': 'Thu Aug 27 20:15:23 2020'}

```
    
#### CP4D.7. Un-deploy application
From CP4D Console, perform these steps.  For more information, see [Deleting an application deployment](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-unregister.html) topic.
1. Continuing from the 'Rolling Average Sample' panel
    - CPD console > Navigation Menu > Analyze > Edge Analytics > Analytics apps > Rolling Average Sample
1. Go to row for the application instance for the edge node that you would like to un-deploy the app from, and select three dots at clear right part of row to see the list of options.
    1. Select 'Delete'
    1. Confirm the Delete          

### Deploying an Edge application using EAM 
        
#### EAM.4. Select Edge Node(s) for development and deployment (via CP4D Console)
To see list of Edge nodes that have been tethered to this CPD instance, do these steps:
1. login in to CPD Console
1. Select Navigation Menu > Analyze > Edge Analytics > Remote systems
    This will display a list of the available nodes.  Select one of the _ieam-analytics-micro-edge-system_ type nodes for the development system.  Also, select one of these for the deployment system.  It can be the same system.

#### EAM.5. Develop / Publish application package 
Use the Secure Shell protocol (ssh) to log in to CP4D Edge node chosen for development and perform the following steps.  For more information, see [Packaging using Edge Application Manager](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-register-by-eam.html) topic.

- Install the OpenShift® command-line interface.
For more information, see [Get Started with the CLI 3.11](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html) or [Getting started with the CLI 4.3](https://docs.openshift.com/container-platform/4.3/cli_reference/openshift_cli/getting-started-cli.html).

- Setup the environment variables

```
    eval export $(cat agent-install.cfg)
    export HZN_EXCHANGE_USER_AUTH= _my_eam_api_key_
    export OCP_USER="cpd-admin-sa"
    export OCP_DOCKER_HOST=_default-route-to-openshift-image-registry_
    export OCP_TOKEN=_cpd-admin-sa_openshift-token_
    export IMAGE_PREFIX=_imagePrefix_   // from build step
```
    
- Login to OpenShift and Docker
```
    oc login _openshift_cluster_url:port_ --token $OCP_TOKEN --insecure-skip-tls-verify=true
    docker login $OCP_DOCKER_HOST --username $OCP_USER --password $(oc whoami -t)
```
- Pull the edge application image to the development node
```
    docker pull $OCP_DOCKER_HOST/$IMAGE_PREFIX/edge-sensorrollingaverage:streamsx
```
- Create a cryptographic signing key pair.
```
    hzn key create "my_company_name" "my_email_address"
```
- Create EAM service project
```
    mkdir rolling_average_sample; cd rolling_average_sample
    hzn dev service new -s rolling-average-service -V 1.0 --noImageGen -i $OCP_DOCKER_HOST/$IMAGE_PREFIX/edge-sensorrollingaverage:streamsx
```  
- Test the service by starting the service, reviewing the container logs, and stopping the service.

```
    hzn dev service start -S
    sudo cat /var/log/syslog | grep edge-sensorrollingaverage[[]
    hzn dev service stop
```

- Publish service

```
    hzn exchange service publish -r "$OCP_DOCKER_HOST:$OCP_USER:$OCP_TOKEN" -f horizon/service.definition.json
```
    
    1. verify rolling-average-service was published and exists in the service list.
    
        hzn exchange service list
        
- Publish pattern

```
    hzn exchange pattern publish -f horizon/pattern.json 
```
    
    1. verify pattern-rolling-average-service pattern was published and exists in this pattern list.
    
    
```
        hzn exchange pattern list
```
            

#### EAM.6. Deploy application package to an Edge node 
Use the Secure Shell protocol (ssh) to log in to CP4D Edge node chosen for deployment and perform the following steps.    For more information, see [Deploying using Edge Application Manager](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-deploy-eam.html) topic. 

- Deploy pattern/service with user inputs.

```
    hzn register -p pattern-rolling-average-service-amd64
    
```
- Verify that application is deployed, by checking for an agreement being created.  This make take a few minutes to show up.
    
```
    hzn agreement list
```
    

#### EAM.7. View the runtime logs (ssh to CP4D Edge node chosen for deployment)

    hzn service log -f rolling-average-service
    
- View log statements
    - This log contains a variety of statements.  The standard println output will be in this log, as well as the output from the trace statements.

```2020-08-27T13:15:23.047502063-07:00 stdout F {'average': 1729.796946015279, 'sensor_id': 'sensor_1', 'period_end': 'Thu Aug 27 20:15:23 2020'}
2020-08-27T13:15:23.047715310-07:00 stdout F {'average': 1321.2858402449626, 'sensor_id': 'sensor_10', 'period_end': 'Thu Aug 27 20:15:23 2020'}
2020-08-27T13:15:23.047765359-07:00 stdout F {'average': 411.0143925640869, 'sensor_id': 'sensor_3', 'period_end': 'Thu Aug 27 20:15:23 2020'}
2020-08-27T13:15:23.047806104-07:00 stdout F {'average': 1788.7616988434274, 'sensor_id': 'sensor_4', 'period_end': 'Thu Aug 27 20:15:23 2020'}

```

        
#### EAM.8. Un-deploy application
For more information, see [Deleting an application deployment](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-unregister.html) topic.
```
        hzn unregister -f
```








- You can deploy this application either by using Cloud Pack for Data Edge Analytics, or by using IBM Edge Application Manager.  The following references will describe the steps needed for each of the methods. 
  - See [Packaging your edge application](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-register-app.html) for details on the packaging step.
  - Next, see [Deploying your edge application](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-deploy.html) for details about deploying your application to one or more edge systems.

#### 5.  Observe the application running on the edge
- This sample application writes output to its application log on the edge.  The rolling averages for each sensor are continuously written to the log.
  - If you deployed using Cloud Pack for Data Edge Analytics, see [Monitoring edge systems and applications with Edge Analytics](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-monitor.html) for detailed instructions on how to view status, logs, metrics, etc. for edge applications.
  - If you deployed using IBM Edge Application Manager, use its facilities to observe your running edge applications.  Application logs can also be accessed directly on the edge systems.
  
Here is an example of the output.
```
2020-08-26T12:57:32.669001408-07:00 stdout F {'average': 1676.468925031888, 'sensor_id': 'sensor_1', 'period_end': 'Wed Aug 26 19:57:32 2020'}
2020-08-26T12:57:32.669061438-07:00 stdout F {'average': 1535.4473696593964, 'sensor_id': 'sensor_10', 'period_end': 'Wed Aug 26 19:57:32 2020'}
2020-08-26T12:57:32.669075576-07:00 stdout F {'average': 1775.567856314266, 'sensor_id': 'sensor_2', 'period_end': 'Wed Aug 26 19:57:32 2020'}
2020-08-26T12:57:32.669097607-07:00 stdout F {'average': 1309.5090881266447, 'sensor_id': 'sensor_3', 'period_end': 'Wed Aug 26 19:57:32 2020'}
```
