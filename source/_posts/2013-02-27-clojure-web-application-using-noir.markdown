---
layout: post
title: "Tutorial - Clojure Web Application Using Noir"
date: 2013-02-27 15:24
comments: true
author: Deepali
categories: 
---

Here is a brief walkthrough of creating a simple website using Noir
framework.


The easiest way to get Noir setup is to use
   [Leiningen](http://google.com). Then execute Following Command In Terminal
```
$ lein new noir my-website
Generating a lovely new Noir project named testing...
```
The above command create a new website in a specified directory.

Created website has following structure :-
```
       /my-website
          --resources/
           --public/
               --css/reset.css
               --img/
               --js/
           --src/
             --my-website/
             --models/
             --views/ common.clj
                     welcome.clj
             server.clj
           --test/
             --my-website/
          project.clj
          README.md
```
Execute the following command :-
```
       lein run   
       Starting server...
       2012-08-16 09:39:22.479:INFO::Logging to STDERR via org.mortbay.log.StdErrLog
       Server started on port [8080].
       You can view the site at http://localhost:8080
       #<Server Server@2206270b>
       2012-08-16 09:39:22.480:INFO::jetty-6.1.25
       2012-08-16 09:39:22.521:INFO::Started SocketConnector@0.0.0.0:8080
```
`lein run` run the -main function in your namespace.

Open the browser and run :-
```
      localhost:8080
```
For creating pages :-
 
 Open `src/views/welcome.clj` file. And create pages in it by refering
following example.

```clojure
  (defpage "/testpage/:user" {:keys [user]}
  [:div user])
```
