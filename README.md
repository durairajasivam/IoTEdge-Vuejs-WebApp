IoTEdge-Vuejs-WebApp
Deploying Vuejs Web app as an Azure IoTedge Module
Create Sample module using IoTEdge Template available in vs code.
Refer here, Incase if you are wondering how to create - 
https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-node-module?view=iotedge-2020-11
Transfer your Vuejs web app src folder to the sample module.
Add Vuejs`s dependencies,devDependencies, etc from package.json to IoT edge module package.json and change the entry point of the app.(for me it is main.js)
Configure container registry with env variables.
then configure your env file with credentials.
Remember to set setDefaultPlatform and setDefaultEdgeRuntimeVersion for your iotedge module.
Configure docker file like shown here (in my case it is Dockerfile.amd64).

Hostconfig
Add Following configration to createoption, this is to allow the web app port access from host system(In docker term, exposing port). basically it will map the port(8080)from inside the container to host port (8085).

The rest is same old Azure IoT edge deployment 🎉.

For new bees, I am going further 
Open your terminal in vs code and login to the container registry ( this is is to push your Docker image to the container) find out the login commands based on your CR.
Connect to your IoTHuB, you can do this in the Vscode itself.
Right-click on the device and deploy your module to your device.
incase if you want to simulate in IoTedge Simulator. then here we go
Right-click on the "deployment.template.json" file and click "Build and Run IoTedge Solution in Simulator" 

Open a command prompt and run "docker ps" to see your module port bindings. for me, I have assigned both sides the same port number (8080)
in your docker dashboard, you can see your module running.
Open your browser and access the Vuejs app using 127.0.0.0:8085 ( for me it is 8080). Voila :) .
Happy Coding :)
Thanks for reading.

