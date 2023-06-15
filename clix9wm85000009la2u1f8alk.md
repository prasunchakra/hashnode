---
title: "Demystifying CORS"
seoTitle: "HTTP headers for CORS explained."
seoDescription: "HTTP headers for CORS explained."
datePublished: Thu Jun 15 2023 15:05:50 GMT+0000 (Coordinated Universal Time)
cuid: clix9wm85000009la2u1f8alk
slug: demystifying-cors
tags: web-development, security, cors

---

In the vast landscape of web development, the ability to seamlessly interact with resources from different domains is crucial. However, this ability must come hand-in-hand with stringent security measures to safeguard users' data and protect against malicious activities. This is where Cross-Origin Resource Sharing (CORS) steps in. But what exactly is CORS, and how do HTTP headers play a pivotal role in this process? In this blog post, we will dive deep into the world of CORS and explore the essential HTTP headers that facilitate secure cross-origin data exchange. So fasten your seatbelts, and let's embark on this enlightening journey!

* **Access-Control-Allow-Origin**: The Access-Control-Allow-Origin header is used by the server to specify which origins are allowed to access its resources. It defines the domains that are granted permission to make cross-origin requests. By setting the appropriate value in this header, you can control the level of access to your resources. It can be set to a specific origin or to `*` to allow any origin.
    
* ```javascript
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Origin', 'https://example.com');
    ```
    
* **Access-Control-Allow-Methods**: The Access-Control-Allow-Methods header specifies the HTTP methods (e.g., GET, POST, PUT, DELETE) that are allowed for cross-origin requests. It provides a mechanism for the server to restrict or allow specific methods for accessing its resources from different origins. It can be set to a comma-separated list of methods or to `*` to allow any method.
    
* ```javascript
    response.setHeader('Access-Control-Allow-Methods', 'GET, POST');
    ```
    
* **Access-Control-Allow-Headers**: The Access-Control-Allow-Headers header defines the list of headers that are allowed in a cross-origin request. It enables the server to specify which custom headers can be used in conjunction with the request. It can be set to a comma-separated list of headers or to `*` to allow any header.
    
* ```javascript
    response.setHeader('Access-Control-Allow-Headers', 'Content-Type');
    response.setHeader('Access-Control-Allow-Headers', 'Authorization');
    ```
    
* **Access-Control-Allow-Credentials**: This header specifies whether credentials such as cookies or authorization headers are allowed when accessing the resource. It can be set to `true` or `false`.
    
* ```javascript
    response.setHeader('Access-Control-Allow-Credentials', 'true');
    ```
    
* **Access-Control-Max-Age**: This header specifies how long the results of a preflight request can be cached. It is specified in seconds.
    
* ```javascript
    // Cache preflight request results for 1 hour (3600 seconds)
    response.setHeader('Access-Control-Max-Age', '3600');
    ```
    
* Here’s a small example of how you can use these headers in your code:
    
* ```javascript
    const express = require('express');
    const app = express();
    
    app.use((req, res, next) => {
      // Set CORS headers
      res.setHeader('Access-Control-Allow-Origin', '*');
      res.setHeader('Access-Control-Allow-Methods', 'GET, POST');
      res.setHeader('Access-Control-Allow-Headers', 'Content-Type');
      res.setHeader('Access-Control-Allow-Credentials', 'true');
      res.setHeader('Access-Control-Max-Age', '3600');
      next();
    });
    
    app.get('/', (req, res) => {
      // Send response with CORS headers
      res.send('Hello World!');
    });
    
    app.listen(3000, () => {
      console.log('Server started on port 3000');
    });
    ```