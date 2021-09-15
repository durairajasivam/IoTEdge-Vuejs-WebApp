# Project Title
IoTEdge-Vuejs-WebApp

## Description

Deploying Vuejs Webapp as a Azure IoTedge Module


Take Note!!! as of now (15/9/2021) - IoT Edge does not support Node.js modules using Windows containers.

# I am not going to talk about iotedge deployment and simulating the modules in details.

Create Sample module using IoTEdge Template available in vscode.

Refere here if you dont know how to create - https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-node-module?view=iotedge-2020-11

Transfer your vuejs web app src folder to sample module

![alt text](https://github.com/durairajasivam/IoTEdge-Vuejs-WebApp/blob/master/modules/images/Src.PNG)

Add vuejs`s dependencies,devDependencies,etc from package.json to iotedge module package.json and change the entry point of the app.(for me main.js)

Configure container registry with env variables

![alt text](https://github.com/durairajasivam/IoTEdge-Vuejs-WebApp/blob/master/modules/images/crconfig.PNG)

then configure your env file with crdentials

![alt text](https://github.com/durairajasivam/IoTEdge-Vuejs-WebApp/blob/master/modules/images/Env.PNG)


## Remeber to set setDefaultPlatform and setDefaultEdgeRuntimeVersion for yuur iotedge module 

Configure docker file like this  (in my case  it is Dockerfile.amd64)
```
# Choose the Image which has Node installed already
FROM node:lts-alpine

# install simple http server for serving static content
RUN npm install -g http-server

# make the 'app' folder the current working directory
WORKDIR /app

# copy both 'package.json' and 'package-lock.json' (if available)
COPY package*.json ./

# install project dependencies
RUN npm install

# copy project files and folders to the current working directory (i.e. 'app' folder)
COPY . .

# build app for production with minification
RUN npm run build

EXPOSE 8080
CMD [ "http-server", "dist" ]
```

# Hostconfig
Add Following configration to createoption, this is to allow the web app port access. basically it will map the port(8080) inside the container to host port (8085) . 
```
"createOptions":  {                
                  "HostConfig": {
                    "PortBindings": {
                      "8080/tcp": [ 
                        { 
                          "HostPort": "8085"
                        } 
                      ] 
                    } 
                  }
                }
```

Login to Container registry from your vscode terminal 

Connect to your IoTHuB 

![alt text](https://github.com/durairajasivam/IoTEdge-Vuejs-WebApp/blob/master/modules/images/IotHub.PNG)

Right click on the "deployment.template.json" file and click "Build and Run IoTedge Solution in Simulator" 

![alt text](https://github.com/durairajasivam/IoTEdge-Vuejs-WebApp/blob/master/modules/images/build%20and%20run.PNG)


Open command prompt and run "docker ps" to see your module port bindings. for me i have assigned bothside same port number (8080)

![alt text](https://github.com/durairajasivam/IoTEdge-Vuejs-WebApp/blob/master/modules/images/dockerps.PNG)

Open your broswer and access the vuejs app using 127.0.0.0:8085 ( for me it is 8080).

![alt text](https://github.com/durairajasivam/IoTEdge-Vuejs-WebApp/blob/master/modules/images/web%20app%20running%20.PNG)

Happy coding :)


