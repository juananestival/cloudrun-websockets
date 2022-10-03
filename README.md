# Intro
This repo explains how to create a websocket server using cloud run. 

## Create the server with this code. 

1. Init npm

```sh
npm init
npm i express 
```
Scripts in package.json

```json
 "scripts": {
    "start": "node index"
  },
```



index.js

```js
const express = require('express');
const app = express();
const http = require('http').Server(app);
const io = require('socket.io')(http);
const port = process.env.PORT || 3000;

app.use(express.static(__dirname + '/public'));

io.on("connection", (socket) => {
    console.log(`Client connected [id=${socket.id}]`);
    socket.emit('server_setup', `Server connected [id=${socket.id}]`);
  });
  
http.listen(port, () => console.log('listening on port ' + port));
```

2. index.html in public folder

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web Socokets Testing Page</title>
    <script src="https://cdn.socket.io/4.5.0/socket.io.min.js" integrity="sha384-7EyYLQZgWBi67fBtVxw60/OWl1kjsfrPFcaU0pp0nAh+i8FD068QogUvg85Ewy1k" crossorigin="anonymous"></script>
</head>
<body>
   
    <p> Open the developer console of the browser to check the logs</p>

    <script type="text/javascript">
        const socketio = io();
        const socket = socketio.on('connection', function() {      
        });
        socketio.on('server_setup', function (data) {
            console.log(data);     
        });   
    </script>
    
</body>
</html>
```

3. Create a project
Enable billing
Enable Cloudrun api
Enable Artifact Registry API
Enable cloudbuild API

set active project
```sh
gcloud config set project <id>
```
4. crun

```sh
gcloud run deploy websocketserver --allow-unauthenticated --source=.
```

```sh
Deploying from source requires an Artifact Registry Docker repository to store
built containers. A repository named [cloud-run-source-deploy] in region
[europe-southwest1] will be created.
```

you will be asked 
API [cloudbuild.googleapis.com] not enabled on project [918232038614]. Would you
 like to enable and retry (this will take a few minutes)? (y/N)?

5. Permissions

Check your organization policy 
constraints/iam.allowedPolicyMemeberDomains. Allow all

then select the service (cloud run) from console On the left hand side will be a panel for permsisions. 
add a princiall called allUsers with cloud run invoker
