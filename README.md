# OpenshiftProjects
# Debugging a Running Node.js App in OpenShift
Date: 15.08.2025  
Prepared by: Latifa Azizi Vakili  
Topic: Changing a Running Container (Chapter 2)

Project Goal:  
Deploy a Node.js application to OpenShift, then modify its source code directly inside a running container using oc CLI tools.  
Verify and observe the changes in real-time using curl all without rebuilding or redeploying the container.

Project structure:
<pre> ``` bash node-debug-app/ ├── server.js ├── package.json ├── Dockerfile ├── node_modules/ └── OpenShift/ ├── BuildConfig # oc start-build triggers this ├── Pods # Running Node.js pods ├── Services # Service exposing the pod └── Routes # Route to access app externally  ``` </pre>
