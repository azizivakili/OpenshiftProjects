## OpenshiftProjects
# Debugging a Running Node.js App in OpenShift

###### Date: 15.08.2025
###### Prepared by: Latifa Azizi Vakili
###### Topic: Changing a Running Container (Chapter 2)

<br>


**Project Goal:**

Deploy a Node.js application to OpenShift, then modify its source code directly inside a running container using oc CLI tools.  
Verify and observe the changes in real-time using curl all without rebuilding or redeploying the container.

### Project structure:
```bash
node-debug-app/
├── server.js
├── package.json
├── Dockerfile
├── node_modules/
└── OpenShift/
    ├── BuildConfig     # oc start-build triggers this
    ├── Pods            # Running Node.js pods
    ├── Services        # Service exposing the pod
    └── Routes          # Route to access app externally
```



### Step 1: Set up the environment

```bash
mkdir node-debug-app && cd node-debug-app
```

### Step 2: Create `server.js`

Run the following command in your terminal to create the `server.js` file:

```bash
cat <<EOF > server.js
const express = require('express');
const app = express();
const port = 3000;

// Define a route
app.get('/', (req, res) => {
  res.send('Helllooo azizi use OpenShifft'); // Intentional typo for debugging
});

// Start the server
app.listen(port, () => {
  console.log(\`App listening at http://localhost:\${port}\`);
});
EOF
```



### Step 3: Create `package.json`

```bash
cat <<EOF > package.json
{
  "name": "node-debug-app",
  "version": "1.0.0",
  "main": "server.js",
  "dependencies": {
    "express": "^4.18.2"
  },
  "scripts": {
    "start": "node server.js"
  }
}
EOF
```


###  Step 4: Create Dockerfile
```bash
cat <<EOF > Dockerfile
FROM node:18
RUN apt-get update && apt-get install -y nano && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY . .
RUN chmod -R 777 /app
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
EOF
```

### Step 5: Build and verify image
```bash
oc start-build node-debug-app --from-dir=. --follow
oc get build
```

#### Example output:
```text
NAME              TYPE     FROM    STATUS     STARTED     DURATION
node-debug-app-2  Docker   Binary  Complete   2 days ago  42s
```

### Step 6: Deploy and expose app
```bash
oc new-app . --name=node-debug-app
oc get pods
oc expose svc/node-debug-app
oc get route
```

#### <span style="color: Blue;">Access The App:</span>

```bash
curl node-debug-app-user-azizi-debugcontainer.apps.akaishi.promos-consult.de
```

#### Output:

``Helllooo azizi use OpenShifft 🚀``


### Step 7: ⚡ Modify app inside container

```bash
oc cp server.js node-debug-app-6748f89fb-hdbqz:/app/server.js
oc delete route node-debug-app
oc expose svc/node-debug-app
```


<h3 style="color: blue;">Rebuild and expose again</h3>


```bash

oc delete all -l app=node-debug-app
oc new-app . --name=node-debug-app
oc expose svc/node-debug-app
```

### Final Check 

```bash
curl node-debug-app-user-azizi-debugcontainer.apps.akaishi.promos-consult.de

```


#### Output:

``Helllooo azizi use OpenShifft 🚀 now i want to bring second change!``




## Summery:
 * Built and deployed your Node.js app on OpenShift

* Modified and tested live changes inside a running container

* Verified updates using curl
