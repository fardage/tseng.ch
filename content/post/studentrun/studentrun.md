+++
author = "Marvin Tseng"
date = 2021-05-25T11:50:29Z
description = ""
draft = false
image = "/2021/05/Screen-Shot-2021-05-22-at-17.58.31.png"
slug = "studentrun"
title = "🏃‍♂️ StudentRun"

+++


Another semester with another software project. This time we decided to code a web-based multiplayer game. In a group of six, we came up with StudentRun. Building a real-time game posed new challenges different from your usual CRUD application. By applying modern development paradigm like scrum, CI/CD, etc., we were able to deliver a fully functional online Jump & Run game.

## Technologies

### JavaScript

To leverage all the benefits of web technologies, we build StudentRun as a Full-Stack JavaScript app. With the runtime environment Node.js in the back-end, communication to the web browser is ensured via a web server and the WebSocket protocol. The initial delivery of HTML, CSS and JavaScript files is done via the Express.js framework. REST-API interfaces enable the transfer of game data in JSON. To meet the real-time requirements of the game, connections between the server and client are established via Socket.IO.The rendering of the game is done in the web browser using the HTML5 gaming framework Phaser. If browser support is available, this framework can render the game elements in WebGL. If the WebGL functionality is not available, an HTML5 canvas is used.

{{< vimeo 554693685 >}}

### Physics engine

Matter.js is a 2D physics engine written in JavaScript. It is used in StudentRun in the backend and frontend. The library offers functionalities to create rigid bodies and model their physical properties like mass, area or density. Processes like collisions, gravity or friction can also be simulated.

### Continuous Integration / Continuous Delivery

The StudentRun code repository is maintained on the GitHub Enterprise environment of the ZHAW. New changes to the application go through standard DevOps process steps.When a developer creates a pull request, the complete program code is verified and tested in a first automated step. Criteria such as test coverage must be met. This is followed by a code review done by one or more members of the development team. Requested changes have to be discussed or are implemented. Once all issues have been resolved, the new changes can be introduced into the main branch. This action triggers the final chain for application deployment. Re-checking through unit tests and an analysis using SonarCloud ensures the quality on the productive instance. The new version of the program code is processed by an "Azure DevOps Self-Hosted Docker Agent" and deployed at a publicly accessible address.

### Architecture

The application can be divided into two components. The back-end module runs the game on the server side, while the front-end module runs the game on the client side. The interaction of the two modules and their entities is illustrated in Figure 1.

![Arch](/studentrun-arch.png)

## Where can I play the game?

Well as long as ZHAW is still hosting the server, it is available [here](http://studentrun.tseng.ch/) ([http://160.85.252.176](http://160.85.252.176)).

Or have a look at our code here: [https://sonarcloud.io/dashboard?id=StudentRun](https://sonarcloud.io/dashboard?id=StudentRun)

