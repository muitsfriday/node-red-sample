# node-red

### วิธี install ด้วย node
อันนี้คือสร้าง server เลย ใช้งานได้ทันที

```javascript
var http = require('http');
var express = require("express");
var RED = require("node-red");

// Create an Express app
var app = express();

// Add a simple route for static content served from 'public'
app.use("/",express.static("public"));

// Create a server
var server = http.createServer(app);

// Create the settings object - see default settings.js file for other options
var settings = {
    httpAdminRoot:"/red", // path ไปที่หน้าแอดมิน
    httpNodeRoot: "/api", // path ไปที่ mocking api
    userDir:"/home/nol/.nodered/", // path ที่ใช้เก็บ config / setting / save
    functionGlobalContext: { }    // enables global context
};

// Initialise the runtime with a server and settings
RED.init(server,settings);

// Serve the editor UI from /red
app.use(settings.httpAdminRoot,RED.httpAdmin);

// Serve the http nodes UI from /api
app.use(settings.httpNodeRoot,RED.httpNode);

server.listen(8000);

// Start the runtime
RED.start();
```

เสร็จแล้วเข้าไปที่
`host:8000/red` สำหรับหน้าสร้าง node
`host:8000/api` เรียก api ที่เราสร้างเอาไว้ใน node-red


Inline-style: 
![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image1.png?raw=true "Logo Title Text 1")

## Node HTTP
Inline-style: 
![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image2.png?raw=true "Logo Title Text 1")

สร้าง endpoint เรียกเข้า โดยกำหนด method, path ได้ สามารถเจาะช่อง param ได้ด้วย :paramname คล้ายๆ express ข้อมูลจะส่งมาทาง `msg.req.params`

ข้อมูลที่ส่งมากะ BODY เช่น POST, PUT จะแนบมากับ `msg.payload` แทน

จากภาพคือการกำหนด endpoint /article/:id id จะส่งต่อผ่าน `msg.req.params.id`
