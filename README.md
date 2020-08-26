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

6. If IEAM will be used to managed edge application lifecycles
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
- The last cell in the notebook will submit a request to your Streams instance to build the application for the edge.  It will take a little while to complete.  While it is running, a progress bar entitled "Building edge image" will display its progress.

  - The build will create a Docker image that can run on an edge system.
  - After a few minutes you should see a message indicating that the build was successful.
  - An additional message will indicate the full path to the Docker image for the application.
  - The image name that this sample produces is:  "edge-sensorrollingaverage:streamsx".

### All of the instructions to this point are not affected by the choice of deployment methods.  However, they will now diverge.  To keep the instructions simpler, there be a separate set of instructions for each deployment method.

### CP4D Deployment method

#### CP4D.4. Package and deploy the application to one or more edge systems using CP4D 
    
- From CP4D Console, perform these steps. For more information, see [Packaging using Cloud Pak for Data](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-register-by-cpd.html) topic. 
    1. Select CPD Console > Navigation Menu > Analyze > Edge Analytics > Analytics apps
    1. Click 'Add Application packages' and fill in these values
        | Field | Value |
        | ----- | ----- |
        | Name | App Control Sample | 
        | Version | 1.0 |
        | Image reference | trades-withtrace:1.0 |
    1. Save
    
     
#### CP4D.5. Deploy application package to an Edge node 
From CP4D Console perform these steps. For more information, see [Deploying using Cloud Pak for Data](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-deploy-cpd.html) topic.  The values for the submission time variables can not be changed at this time.
1. Continuing from the 'Analytics apps' panel
    - CPD Console > Navigation Menu > Analyze > Edge Analytics > Analytics apps
1. Go to end of the row with "App Control Sample" and click on three dots to open list of options, and select 'Deploy to edge'
    1. When the list of remote systems is displayed, check the box next to the remote system you want to deploy to.
    1. Select 'Deploy' option.
1. To verify that the app was deployed successfully, select the "App Control Sample"
    1. Verify that there is an application instance for the deployment to your chosen system.

#### CP4D.6.  Observe the application running on the edge using CP4D
From CP4D Console, perform these steps.  See [Monitoring edge systems and applications with Edge Analytics](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-monitor.html) for detailed instructions on how to view status, logs, metrics, etc. for edge applications.
1. Continuing from the 'App Control Sample' panel
    - CPD console > Navigation Menu > Analyze > Edge Analytics > Analytics apps > app control sample
1. Go to row for the application instance for the edge node that you would like to see log for, and select three dots at clear right part of row to see the list of options.
    1. Select 'Download logs'.
1. Unzip downloaded log package.
1. Open up app-control-sample-xxxx.log file
    - This file contains a variety of statements.  The standard println output will be in this log, as well as the output from the trace statements.  Search for "USER-NAME" for example of println output. You should see the value for 'yourName' that you previously entered in to the application. The statements added due to the application trace statements will contain "#splapptrc".  
    
    - Here is a snippet of the log. Notice that the values for the input variables that were supplied made it to the application and were output to this log file. (e.g. MyFavoriteFootballTeams). 
    Also, notice that the DEBUG-LEVEL message was not in the log.  This means the STREAMS_OPT_TRACE_LEVEL runtime-option that set the level to INFO was received and acted upon by the application, so as only trace statements of info level were accepted in the file. 

```
2020-08-19T10:07:10.064038778-07:00 stdout F 19 Aug 2020 17:07:10.063+0000 [56] INFO #splapptrc,J[0],P[0],PrintAvPrice M[TradesAppCloud_withLogTrace.spl:appTrc:82]  - mySubmissionTimeVariable_string =MyFavoriteFootballTeams
2020-08-19T10:07:10.066033579-07:00 stdout F 19 Aug 2020 17:07:10.063+0000 [56] INFO #splapptrc,J[0],P[0],PrintAvPrice M[TradesAppCloud_withLogTrace.spl:appTrc:83]  - mySubmissionTimeVariable_listOfStrings var: 
2020-08-19T10:07:10.066033579-07:00 stdout F 19 Aug 2020 17:07:10.064+0000 [56] INFO #splapptrc,J[0],P[0],PrintAvPrice M[TradesAppCloud_withLogTrace.spl:appTrc:85]  -    String element: Vikings
2020-08-19T10:07:10.066033579-07:00 stdout F 19 Aug 2020 17:07:10.064+0000 [56] INFO #splapptrc,J[0],P[0],PrintAvPrice M[TradesAppCloud_withLogTrace.spl:appTrc:85]  -    String element: Packers
2020-08-19T10:07:10.066033579-07:00 stdout F 19 Aug 2020 17:07:10.065+0000 [56] INFO #splapptrc,J[0],P[0],PrintAvPrice M[TradesAppCloud_withLogTrace.spl:appTrc:85]  -    String element: Lions
2020-08-19T10:07:10.066033579-07:00 stdout F 19 Aug 2020 17:07:10.065+0000 [56] INFO #splapptrc,J[0],P[0],PrintAvPrice M[TradesAppCloud_withLogTrace.spl:appTrc:85]  -    String element: Bears

2020-08-19T10:07:10.066033579-07:00 stdout F This sample is being is being tried out by: USER-NAME=  yourName


```
    
#### 5. Un-deploy application
From CP4D Console, perform these steps.  For more information, see [Deleting an application deployment](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-unregister.html) topic.
1. Continuing from the 'App Control Sample' panel
    - CPD console > Navigation Menu > Analyze > Edge Analytics > Analytics apps > App Control Sample
1. Go to row for the application instance for the edge node that you would like to un-deploy the app from, and select three dots at clear right part of row to see the list of options.
    1. Select 'Delete'
    1. Confirm the Delete          

### EAM Deployment method

#### EAM.4. Package and deploy the application to one or more edge systems using EAM

#### EAM.5.  Observe the application running on the edge using EAM





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
