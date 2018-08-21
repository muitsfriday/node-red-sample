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


![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image1.png?raw=true "Logo Title Text 1")

## Node HTTP
![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image2.png?raw=true "Logo Title Text 1")

สร้าง endpoint เรียกเข้า โดยกำหนด method, path ได้ สามารถเจาะช่อง param ได้ด้วย :paramname คล้ายๆ express ข้อมูลจะส่งมาทาง `msg.req.params`

ข้อมูลที่ส่งมากะ BODY เช่น POST, PUT จะแนบมากับ `msg.payload` แทน

จากภาพคือการกำหนด endpoint /article/:id id จะส่งต่อผ่าน `msg.req.params.id`


## สร้าง Response
![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image3.png?raw=true "Logo Title Text 1")

สร้าง http response โดย node นี้จะนำ msg.payload ตอบออกไปให้ สามารถ set status code / header ได้


## แก้ไข response
การจะแก้ไข response ที่ออกไป ใช้วิธีเชื่อม node **function**

![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image4.png?raw=true "Logo Title Text 1")

การเขียน fuuction เขัียนได้แบบ javascript เลย จะต้อง return msg ที่มี req, res ออกไป ไม่งั้น node http / http-response จะทำงานไม่ได้

เมื่อเขียน function แล้วให้เชื่อมเส้นใหม่ โดย ลากจาก HTTP ---> Function ---> HTTP response
ดังตัวอย่าง 

## ทางแยกเพื่อสร้าง 404

สร้าง node switch มีหน้าที่ทำทางแยกของงานตามเงื่อนไขของ msg ในที่นี้เราอยากรู้ว่า msg.payload.data เป็น null ไหม ถ้าไม่ null ให้ไปทางออกที่ 1 ไม่งั้นไปทางออกที่ 2

![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image5.png?raw=true "Logo Title Text 1")


ในทางออกที่ 1 ให้เชื่อมกับ http response ปกติ
ส่วนทางออกที่ 2 ให้ไปยัง http response ที่ set status 404 เอาไว้


## Deploy 

เมื่อพร้อมแล้วก็กด Deploy แล้วลองเทสได้เลย

เมื่อเข้าไปที่ /api/article/1 จะได้ผลลัพธ์ดังนี้

![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image6.png?raw=true "Logo Title Text 1")

เมื่อเข้าไปที่ /api/article/2
จะไม่พบผลลัพธ์และแสดง status code เป็น 400

![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image7.png?raw=true "Logo Title Text 1")


## node  อื่นๆที่น่าสนใจ
### Debug
debug เอาไว้ดูข้อความ สามารถดูได้ทั้ง msg หรือจะดูแค่ payload ก็ยังได้

setting ของ node debug เชื่อมไว้กับ http node ดักข้อความจาก HTTP
![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image8.png?raw=true "Logo Title Text 1")

เมื่อมีข้อความ เข้ามา สามารถดู msg ได้จากช่อ debug

![alt text](https://github.com/muitsfriday/node-red-sample/blob/master/images/image9.png?raw=true "Logo Title Text 1")