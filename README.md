Node.js can run JavaScript anywhere. Google developed V8 machine this machine used by Node.js takes in JavaScript code compiles it and compiles to machine code.

//test.js

const fs = require('fs');
fs.writeFileSync('hello.txt', 'Hello from Node');

import file using 'require('fs')' and will write file to harddrive 'writeFileSync' and the first argument in 'writeFileSync' is path to the file allowing with the file name and second argument is the content of File created here 'hello.txt'.

JavaScript on Server

In Server database will use to store and fetch database, authentication


REPL

REPL stands for R for Read User input, E for val which evaluates user input, P for Print output, L for loop waiting for new input.
Execute Node.js code using REPL


How web works

When user/client/browser enter sa web page url say "http://my-page.com", Browser will look for a domain Server and the browser will sent a url request to Server. Response (eg: HTML page) from Sever to Client. Request and response are through HTTP or HTTPS protocols
HTTP is a protocol for transferring data which is understood by browser and server.
HTTPS = HTTP + Data Encryption for data transmission.



Core Modules

http (launch a sever or send requests, https (http and https useful for creating http requests and responses), fs, path (constructing path to file or file system), os



In Node.js, require() is a built-in function used to import modules, which are self-contained files containing JavaScript code designed to perform a specific task.

Example

const http = require("http");
function rqListener(req, res) {}
http.createServer(rqListener);

imports http module and http object creates a server using 'http.createServer();
'createServer()' takes in argument requestListener where requestListener is a function (here rqListener) that gets invoked for each incoming request. 'rqListener' takes in 2 arguments where first argument is request of type IncomingMessage and second argument is response which is of type ServerResponse. Node.js gives an object (here 'req') that represents incoming request and allow to read data from that request as the first argument and second argument Node.js given an object (here 'res') as response to send requests whoever has sent the request.
'rqListener' will be executed for each incoming requests since given in 'createServer()' . 'rqListener' is executed each time when a requests reaches server.

'createServer()' can be written like follows also


const http = require("http");
const server = http.createServer(function (req, res){
    
});
server.listen(3000);

Can be also written like this:-


const http = require("http");
const server =  http.createServer( (req, res) => {});
server.listen(3000);


'listen()' will tell Node.js to listen before exiting. 'listen()' will have argument where argument is to listen to the port. 



Node.js Program Lifecycle


const http = require("http");
const server =  http.createServer( (req, res) => {});
server.listen(3000);

When we written the above code in app.js file  and run the command 'node app.js' then script gets started. Where Node.js went through entire file where parse the code, register variables and functions. Event loop is a process managed by Node.js which keeps on running as long there is work to do or event listeners are registered. One event listener we registered and not unregistered is  (req, res) => {} inside 'createServer. All code managed by event loop and Node.js uses this event loop for all kind of stuff not only to manage server, but also while accessing a database , register a function and should be executed. Node.js executes this event driven approach because it uses single threaded JavaScript. the entire node process basically uses one thread on our computer its running on. Event loop triggers when an event has taken place. If we want to unregister and then it can be done by process.exit (then the process would end in event loop )

example:-

const http = require("http");
http.createServer((req, res) => {
    console.log(req);
    process.exit();
});

Process exit basically hard exited our event loop and therefore the program shuts down because there was no more work to do and it basically closed the program and gave control back to terminal.

In 'req' there is 'headers' which is metadata which is added to request (header data include port where server is listening the browser where its listening if its google chrome, header information added to request, which kid of response we accept like 'text/html' ), http version added.

In 'req' we have url which here url is '\' here bacuse everything that comes after 'http:localhost:3000' 
If the http:localhost:3000/test was done in the browser then we get 'req.url' as '/test'.

In 'req' we have method is GET

Example:-

const http = require("http");

http.createServer((req, res) => {
  console.log(req.url, req.method, req.headers);
  res.setHeader("Content-Type", 'text/html');
  res.write("<html>");
  res.write("<head><title>My First Node.js page</title></head>");
  res.write("<body>My first Node.js Body</body>");
  res.write("</html>");
  res.end();
});


Here 'res' is set of type text/html.

res.write writes a chunk of data. Here to 'res' a chunk of data is written with html , head , title and body tags. For the res.write to come to an end we need to give res.end() at last where we finish writing into the response ('res'). res.end() will sent back to the client



const http = require("http");

const server = http.createServer((req, res) => {
  const url = req.url;
  if (url === "/") {
     res.write('<html>');
    res.write('<head><title>Enter Message</title></head>');
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message"><button>Send</button> </form></body>'
    );
    res.write('</html>');
    return res.end();
  }
  res.setHeader("Content-Type", "text/html");
  res.write("<html>");
  res.write("<head><title>My First Node.js page</title></head>");
  res.write("<body>My first Node.js Body</body>");
  res.write("</html>");
  res.end();
});

server.listen(3000);



Here url === "/" then form with action redirected the form values to '/message'

name="message" in input tag . Will add any input data to the request and make it accessible via the assigned name.

return res.end() will stop res.write execution 


In the below code if the form data we entered in input tag in the above is redirected to '/' with statuscode 302 and placing the data of the form in 'message.txt'


if (url === "/message" && method === "POST") {
    fs.writeFileSync("message.txt", "DUMMY");
    res.statusCode = 302;
    res.setHeader("Location", "/");
    return res.end();
  }


Full example:

const http = require("http");
const fs = require("fs");

const server = http.createServer((req, res) => {
  const url = req.url;
  const method = req.method;
  if (url === "/") {
    res.write("<html>");
    res.write("<head><title>Enter Message</title></head>");
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message"><button>Send</button> </form></body>'
    );
    res.write("</html>");
    return res.end();
  }
  if (url === "/message" && method === "POST") {
    fs.writeFileSync("message.txt", "DUMMY");
    res.statusCode = 302;
    res.setHeader("Location", "/");
    return res.end();
  }
  res.setHeader("Content-Type", "text/html");
  res.write("<html>");
  res.write("<head><title>My First Node.js page</title></head>");
  res.write("<body>My first Node.js Body</body>");
  res.write("</html>");
  res.end();
});

server.listen(3000);




Streams and Buffers

An incoming request include a stream of data where Node.js read data in chunks and after reading this chunks of data the stream of data is fully parsed. For example when a file being uploaded and this will considerably longer therefore streaming of data and reading data in chunks by Node.js that will allow to start writing this in your disk or hard drive, so to your hard drive when your app runs make the node app runs on server, while the data is coming in. So need not have to parse the entire file which will take a lot of time and you have to wait for being uploaded before you can do anything with it.
The stream of data we can start working on the data early and organize these data in chunks by buffer. A buffer is like a bus stop. If you consider buses they are always driving but for customers or users being able to work with them to climb on the bus and leave the bus you need bus stops where you well can track the bus basically and that is what a buffer is. 
A buffer is a construct which allows you to hold multiple chunks and work with them before they are released once you are done.

Request should listen to an event listener ' req.on('data', (chunk)=>{
body.push(chunk)}) ' here 'on' listens to some events and the event I need to listen is the data event. Data event is fired whenever new chunk is ready to be read. Second argument to 'on' is a function that should be executed for every data event and the function receives a chunk of data.

const body = [];
    req.on("data", (chunk) => {
      body.push(chunk);
    });
   
 After data is parsed we can event listener of 'on' with end. (eg: -  req.on("end", () => {
      const parseBody = Buffer.concat(body).toString();
      const message = parseBody.split("=")[1];
      fs.writeFileSync("message.txt", message);   
 });


Here Buffer will interact with chunks of data and concat the data and convert to string and toString conversion is donr because 'body' is coming as text.





Full example:-

const http = require("http");
const fs = require("fs");

const server = http.createServer((req, res) => {
  const url = req.url;
  const method = req.method;
  if (url === "/") {
    res.write("<html>");
    res.write("<head><title>Enter Message</title></head>");
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message"><button>Send</button> </form></body>'
    );
    res.write("</html>");
    return res.end();
  }
  if (url === "/message" && method === "POST") {
    const body = [];
    req.on("data", (chunk) => {
      body.push(chunk);
    });
    return req.on("end", () => {
      const parseBody = Buffer.concat(body).toString();
      const message = parseBody.split("=")[1];
      fs.writeFileSync("message.txt", message);
      res.statusCode = 302;
      res.setHeader("Location", "/");
      return res.end();
    });
  }
  res.setHeader("Content-Type", "text/html");
  res.write("<html>");
  res.write("<head><title>My First Node.js page</title></head>");
  res.write("<body>My first Node.js Body</body>");
  res.write("</html>");
  res.end();
});

server.listen(3000);



Here in   fs.writeFileSync("message.txt", message);  'writeFileSync' denotes to synchronously write data to the file 'message.txt'. So until this line gets executed (i.e   fs.writeFileSync("message.txt", message);  ) it will block code execution further until the file 'message.txt' is created. But if the data to be written in the file (here message.txt ) is too long and size of file is too high then while writing to file will block the other code execution if we use 'writeFileSync' inorder to resolve this issue we use 'writeFile' instead (so other execution of code will not be blocked). 'writeFile' contains 3 arguments first argument is the path to the file including the file name, second argument is the data to be added to the file, third argument is the callback function which will be executed after the file is created and data is embedded into the file if success, otherwise the callback function triggers an error.

Example:-

fs.writeFile('message.txt', message, (err) => {
      res.statusCode = 302;
      res.setHeader("Location", "/");
      return res.end();
});



But Node.js is usually asynchronous in nature because when one task is under process it doesn't wait to complete that execution instead it will start executing next task and comes back when the previous task is completed.





Single Thread, Event Loop and Blocking Code


Node.js uses only 1 JavaScript thread. A thread is a process in your operating system.
For example: Let's say we have some code which accessed the file system. But when working with files often is a task that takes longer because files can be very big and it doesn't complete instantly, there fore if we are doing this upon an incoming request, a second request might have to wait because we are not able to handle it yet or it even gets declined. So, webpage is down for that user. Event loop is already started by Node.js when your program starts, user need not have to explicitly start the Event Loop starts when the user runs the program. Event loop is responsible for running the code when a certain event occurs, it's aware of all these callbacks and basically well execute said code. Even though file operation is important its operation is not handled by event loop, just the callback that we might have defined on write file once its done that code will be handled by event loop but that code will finish fast. So, event loop will only handle callbacks that contain fast finishing code. Instead our file system operation and a couple of other long taking operations are sent to a worker pool which is also spin up and managed by Node.js automatically. this worker pool is responsible for all heavy lifting. This worker pool is kind of totally detached of your JavaScript code and it runs on different threads, it can spin up multiple threads. Worker pool is closely intervened with your operating system on which app is running on. If there is task to done something on a file then a worker from the worker pool will take care and will do the job which is totally detached from your code and from the request and from the event loop. The one connection with the event loop though is once the worker is done, as per the example once we read a file , it will trigger the callback for that read file operation and since the event loop is responsible for the events and the callbacks this will in end up in the event loop. So the node.js will then basically execute the appropriate callback.


Event Loop

Event loop is a loop which is run or started by Node.js that keeps Node.js process running and event loop handles all the callbacks and it has a certain order in which it goes through the callbacks. Event loop at the beginning of each new iteration it checks if there are any timer callbacks it should execute. If we haven't set up any timers yet but there is setTimeout and setIntreval. In Node.js we can set a timer and basically you set a timer and always pass a method, a function that should be executed once the timer completes and Node.js is aware of this and at the beginning of each new loop iteration, it executes any due timer callbacks, so any callbacks that have to be executed because  a timer completes. Then as a next step, it checks other callbacks for example if we had write or read file, we might have a callback because that operation finished and it will then also execute these callbacks. Input/output operations that typically is file operations but can also be network operations which takes long blocking taking operations. If there are too many outstanding callbacks, it will continue its loop iteration and postpone these callbacks to the next iteration to execute them. After working on these open callbacks and after finishing them all, it will enter a pull phase. The pull phase is basically a phase where Node.js will look for new IO events and basically do its best to execute their callbacks immediately if possible. But if its not possible, it will defer the execution and basically register this as a pending callback. It will check if there is any timer callbacks due to be executed and if timer exists then it will jump to timer phase and execute them right away, so it can actually jump back there and not finish the iteration, other wise it will continue. set immediate callbacks will be executed in a so called check phase. Set immediate is a bit like setTimeout and setIntreval, just that will execute immediately but always after any open callbacks have always been executed ( at least finish open callbacks that were due to be handled in that current iteration). At end of each iteration cycle and now Node.js will execute all close event callbacks, so if you registered any close events , this would be the point of time where nodejs executes their appropriate callbacks. We exit the whole Node.js program but only if there are no remaining event handlers which are registered. Node.js keeps track of its open event listeners and it basically has a counter, references or refs which it increments by 1 for every new callback that is registered, for every new event listener that is registered, so every new future work that it has to do and it reduces counter by 1 for every event listener that it doesn't need anymore, every callback it finished and since in a server environment we create a server with create server and then listen to incoming requests with listen, this is an event which never is finished by default and therefore we always have at least one reference and therefore we don't exit in a normal node web server program.




Splitting app.js and route.js


app.js

const http = require("http");
const routes = require("./route");
const server = http.createServer(routes);
server.listen(3000);



route.js

const fs = require("fs");

const requestHandler = (req, res) => {
  const url = req.url;
  const method = req.method;
  if (url === "/") {
    res.write("<html>");
    res.write("<head><title>Enter Message</title></head>");
    res.write(
      '<body><form action="/message" method="POST"><input type="text" name="message"><button>Send</button> </form></body>'
    );
    res.write("</html>");
    return res.end();
  }
  if (url === "/message" && method === "POST") {
    const body = [];
    req.on("data", (chunk) => {
      body.push(chunk);
    });
    return req.on("end", () => {
      const parseBody = Buffer.concat(body).toString();
      const message = parseBody.split("=")[1];
      fs.writeFile("message.txt", message, (err) => {
        res.statusCode = 302;
        res.setHeader("Location", "/");
        return res.end();
      });
    });
  }
  res.setHeader("Content-Type", "text/html");
  res.write("<html>");
  res.write("<head><title>My First Node.js page</title></head>");
  res.write("<body>My first Node.js Body</body>");
  res.write("</html>");
  res.end();
};

module.exports = requestHandler;




To export we use 'module.exports'.




'  npm init ' command used to create package.json file


Node packages

Node packages include express, body-parser

eg:

npm install nodemon --save-dev

this will install nodemon as production dependency. This above command will give package-lock.json and node_modules.

Delete 'node_modules' and do 'npm install' again which will create dependencies and 'node_modules'

In 'package.json' and in 'scripts' session add "start" : "node app.js" and then change 'start to "start" : "nodemon app.js" and run in the terminal command 'npm start'

