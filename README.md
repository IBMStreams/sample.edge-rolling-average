# sample.edge-rolling-average

This sample contains a simple Python application that you can quickly build for the edge, deploy to the edge, and observe the application as it runs on the edge.  The application computes a rolling average of a value for a group of sensors.  For the purposes of this sample, all the sensor data is generated.

## Before you begin

1. [Install IBM Cloud Pak for Data (CP4D) 3.0.1](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/install/install.html)
2. [Install IBM Edge Analytics beta service on CP4D](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/install.html) and [setup edge systems](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/admin.html)
3. [Install IBM Streams 5.4.0 service on CP4D](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/streams/install-intro.html)
4. [Install Watson Studio service on CP4D](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/wsj/install/install-ws.html)
5. [Provision a Streams instance](https://www.ibm.com/support/producthub/icpdata/docs/content/SSQNUZ_current/cpd/svc/streams/provision.html#provision)

## Instructions

#### 1. Copy this notebook into an IBM Cloud Pak for Data project
- Copy the GitHub URL of the Edge-RollingAverage.ipynb file.
- Create a new, empty project or open an existing project in IBM Cloud Pak for Data.
- From your project, click "Add to project" and select "Add notebook".
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
  - The image name that this sample produces is:  "edge-sensorrollingaverage:streamsx"

#### 4. Package and deploy the application to one or more edge systems
- You can deploy this application either by using Cloud Pack for Data Edge Analytics, or by using IBM Edge Application Manager.  The following references will describe the steps needed for each of the methods. 
  - See [Packaging your edge application](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-register-app.html) for details on the packaging step.
  - Next, see [Deploying your edge application](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-deploy.html) for details about deploying your application to one or more edge systems.

#### 5.  Observe the application running on the edge
- This sample application writes output to its application log on the edge.  The rolling averages for each sensor are continuously written to the log.
  - If you deployed using Cloud Pack for Data Edge Analytics, see [Monitoring edge systems and applications with Edge Analytics](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.0.1/svc-edge/usage-monitor.html) for detailed instructions on how to view status, logs, metrics, etc. for edge applications.
  - If you deployed using IBM Edge Application Manager, use its facilities to observe your running edge applications.  Application logs can also be accessed directly on the edge systems.
