# WebSocket Project with Node.js and HTML

This project demonstrates a simple WebSocket server using Node.js and a client built with HTML and JavaScript. The WebSocket server listens for incoming connections, and the client can send and receive messages.

## Features
- A WebSocket server that listens on `ws://localhost:8080`.
- A simple HTML client that can connect to the server and send messages.
- The server echoes the received messages back to the client.

## Prerequisites
- Node.js installed (which includes npm).
- A web browser for running the client.

## Installation

1. Clone this repository or download the files.
2. Navigate to the project directory:
   ```sh
   cd websocket_project
   ```
3. Initialize a Node.js project and install the required dependencies:
   ```sh
   npm init -y
   npm install ws
   ```

## Running the WebSocket Server

1. Create a file named `server.js` and paste the following code:
   ```javascript
   const WebSocket = require('ws');
   const server = new WebSocket.Server({ port: 8080 });

   server.on('connection', (socket) => {
     console.log('Client connected');

     socket.on('message', (message) => {
       console.log(`Received: ${message}`);
       // Echo the message back to the client
       socket.send(`Server received: ${message}`);
     });

     socket.on('close', () => {
       console.log('Client disconnected');
     });
   });

   console.log('WebSocket server is running on ws://localhost:8080');
   ```

2. Run the server:
   ```sh
   node server.js
   ```
   The server will now be running at `ws://localhost:8080`.

## Creating the WebSocket Client

1. Create a file named `client.html` and paste the following code:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>WebSocket Client</title>
   </head>
   <body>
       <h1>WebSocket Client</h1>
       <button id="connectButton">Connect to Server</button>
       <input type="text" id="messageInput" placeholder="Enter message">
       <button id="sendButton">Send Message</button>
       <div id="output"></div>

       <script>
           let socket;
           const connectButton = document.getElementById('connectButton');
           const sendButton = document.getElementById('sendButton');
           const messageInput = document.getElementById('messageInput');
           const output = document.getElementById('output');

           connectButton.addEventListener('click', () => {
               socket = new WebSocket('ws://localhost:8080');

               socket.onopen = () => {
                   output.innerHTML += '<p>Connected to server</p>';
               };

               socket.onmessage = (event) => {
                   output.innerHTML += `<p>Server: ${event.data}</p>`;
               };

               socket.onclose = () => {
                   output.innerHTML += '<p>Disconnected from server</p>';
               };
           });

           sendButton.addEventListener('click', () => {
               const message = messageInput.value;
               if (socket && socket.readyState === WebSocket.OPEN) {
                   socket.send(message);
                   output.innerHTML += `<p>You: ${message}</p>`;
               } else {
                   output.innerHTML += '<p>Cannot send message. Not connected to server.</p>';
               }
           });
       </script>
   </body>
   </html>
   ```

2. Open `client.html` in your web browser.
3. Click the "Connect to Server" button, enter a message, and click "Send Message" to interact with the server.

## Summary
- **Server**: Use `server.js` to create a WebSocket server with Node.js.
- **Client**: Use `client.html` to connect to the server, send messages, and receive responses.

## License
This project is distributed under the MIT license.
