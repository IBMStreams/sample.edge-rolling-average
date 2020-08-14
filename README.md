# (UNDER CONSTRUCTION - This sample and its instructions are under development) 
# sample.edge-rolling-average

This sample contains a simple Python application that you can quickly build for the edge, deploy to the edge, and observe the application as it runs on the edge.  The application computes a rolling average of a value for a group of sensors.  For the purposes of this sample, all the sensor data is generated.

## Instructions

#### 1. Copy this notebook into an IBM Cloud Pak for Data project
- Copy the GitHub URL of the Edge-RollingAverage.ipynb file.
- Create a new project or open an existing project in IBM Cloud Pak for Data.
- From your project, click "Add notebook".
- Select the "From URL" tab, and paste the URL in the "Notebook URL" field.
- Click "Create notebook".

#### 2. Edit the notebook to specify the name of your Streams instance
- Find the line of code in the last cell in the notebook that has a comment instructing you to replace a value with the name of your Streams instance.
- Please substitute the name of your instance for the value that is in the sample notebook.
- Save your notebook.

#### 3. Run the notebook
- Run each cell in the notebook.
- The last cell in the notebook will submit a request to your Streams instance to build the application for the edge.
  - The build will create a Docker image that can run on an edge system.
  - After a few minutes you should see a message indicating that the build was successful.
  - An additional message will indicate the full path to the Docker image for the application.
  - The image name that this sample produces is:  "edge-sensorrollingaverage:streamsx"

#### 4. Package and deploy the application to one or more edge systems
- Two different deployment methods for packaging and deploying applications are supported:
  - See [Packaging your edge application](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-edge/usage-register-app.html) for details on the packaging step.
  - Next, see [Deploying your edge application](https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-edge/usage-deploy.html) for details about deploying your application to one or more edge systems.
  
#### 5.  Observe the application running on the edge
- This sample application writes output to its application log on the edge.  The rolling averages for each sensor are continuously written to the log.
  - If you deployed using Cloud Pack for Data Edge Analytics, see [Monitoring edge systems and applications with Edge Analytics](
https://www.ibm.com/support/knowledgecenter/SSQNUZ_3.5.0/svc-edge/usage-monitor.html) for detailed instructions on how to view status, logs, metrics, etc. for edge applications.
  - If you deployed using IBM Edge Application Manager, use its facilities to observe your running edge applications.  Application logs can also be accessed directly on the edge systems.
  
  
   
