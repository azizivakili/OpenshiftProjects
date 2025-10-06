## OpenshiftProjects
# Debugging a Running Node.js App in OpenShift
Date: 15.08.2025  
Prepared by: Latifa Azizi Vakili  
Topic: Changing a Running Container (Chapter 2)

Project Goal:  
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

### Step 2: Create server.js

cat <<EOF > server.js
const express = require('express');
const app = express();
const port = 3000;
app.get('/', (req, res) => {
 res.send('Helllooo azizi use OpenShifft  '); // Intentional typo for debugging
});
app.listen(port, () => {
 console.log(\`App listening at http://localhost:\${port}\`);
});
EOF
