---
layout: post
title: "Tutorial 1 - Clojure Web Application Using Noir"
date: 2013-02-27 15:24
author: Deepali
categories: 
---

Here is a brief walkthrough of creating a simple website using Noir
framework.

#### We are going to cover following points in this tutorial :

 * Tutorial 1 for :-
   * Create simple website using Noir.
   * Create Pages.
   * Create Pages for User Interface.
   * Database Connection.

Now we will create a simple finance manager blog.

The easiest way to get Noir setup is to use
   [Leiningen](https://github.com/technomancy/leiningen/). Then execute Following Command In Terminal
```
$ lein new noir finance_manager
Generating a lovely new Noir project named testing...
```
The above command create a `finance_manager website` in a specified directory.

Created website has following structure :-
```
       /finance_manager
          --resources/
           --public/
               --css/reset.css
               --img/
               --js/
           --src/
             --finance-manager/
             --models/
             --views/ common.clj
                     welcome.clj
             server.clj
           --test/
             --finance_manager/
          project.clj
          README.md
```

The `project.clj` file is used for building the application and
managing dependencies by Leiningen.

Under the `src` folder, we have the folder  `finance_manager` which contains `server.clj`. This file contains the entry point to our application. It loads up all the views and provides a main function which can be used to start the application.

The `models` folder is used to keep the data such as code for the database access and table management. The `views` folder contains the namespaces describing the pages of our application and their supporting code. The template contains `common.clj` which provides a basic layout and any code shared between the pages. The `welcome.clj` is the namespace where an example page is defined and you can create your own pages under this namespace.

The `resource/public` folder contains the stylesheets and javasript.

Execute the following command :-
```
       $ lein run   
       Starting server...
       2012-08-16 09:39:22.479:INFO::Logging to STDERR via org.mortbay.log.StdErrLog
       Server started on port [8080].
       You can view the site at http://localhost:8080
       #<Server Server@2206270b>
       2012-08-16 09:39:22.480:INFO::jetty-6.1.25
       2012-08-16 09:39:22.521:INFO::Started SocketConnector@0.0.0.0:8080
```
`lein run` run the -main function in our finance_manager namespace.

Open the browser and run :-
```
      localhost:8080
```
For creating pages :-
 
 Open `src/views/welcome.clj` file. And create a homepage of the
website.

 For creating pages `defpage` macro is used which create a Compojure
route for the specified url. `defpage` has following syntax.
```clojure
  (defpage url params content)
``` 

```clojure
  (defpage "/homepage" []
  [:h3 "Finance Manager"])
```

Open the browser and run the following
```clojure
  localhost:8080/homepage
```
Design a form which will provide the User Interface to add the Monthly
Budget.
```clojure
  (defpage "/addbudget" {:keys [error]}
   (common/layout
     [:h2 "Add Monthly Budget"]
     [:div.error error]
     (form-to [:post "/addbudget"]
        (label "budget_date" "Select Budget Date")
        (text-field "budget_date")
        [:br]
        (label "budget_amt" "Budget Amount")
        (text-field "budget_amt")
        [:br]
        (submit-button "Add Budget")
        (reset-button "Cancel")
     )
   )
)
```

Design the another page for POST which contain the server code to handle
the input coming from addbudget page.
```clojure
 (defpage [:post "/addbudget"] budget
   (common/layout
     [:div (:budget_date budget)]
     [:div (:budget_amt budget)]
   )
 )
```

The above page simply display the values enter by the user on the page.

Open the browser and run
```clojure
  localhost:8080/addbudget
```
This will display the page containing UI to add budget values.

To save the values in the database we have to create the databse and
setup the connection with the database.

Follow the following steps for Database Access.

There are several Clojure libraries availale for dealing with databse.
In this tutorial we are using `clojure.data.jdbc` and MySql databse.

We will create a new namespace under the `src/finance_manager/models`.
We call this namespace db. The namespace will live in a file called
`db.clj` under `src/finance_manager/models` directory.

```clojure
(ns finance_manager.models.db)

(require '[clojure.java.jdbc :as sql])
```

Now we will define our database connection in this file.

```clojure
(def db
  {:subprotocol "mysql"
   :subname "//localhost:3306/finance_manager_db"
   :user "username"
   :password "private"})
```

Now add dependencies in `project.clj` file for database connection.
```clojure
[org.clojure/java.jdbc "0.2.3"]
[mysql/mysql-connector-java "5.1.6"]
```

Create a table for budget in `finance_manager_db`
```clojure
(defn create-budget-table []
  (sql/with-connection
      db
      (sql/create-table
        :budget
        [:id :serial "PRIMARY KEY"]
        [:budget_date "date"]
        [:budget_amt "double"]
      )
    )
)
```

Now open `src/finance_manager/server.clj` file.

Write a function that call this `create-budget-table` function. and call
that function in -main function.
```clojure
(:require [finance_manager.models.db :as db])
(defn init
  []
  (db/create-budget-table)
)

(defn -main []
  (init))
```

Now start REPL session and execute following command in it.
```clojure
$ lein repl
nREPL server started on port 53376
REPL-y 0.1.9
Clojure 1.4.0
    Exit: Control+D or (exit) or (quit)
Commands: (user/help)
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
          (user/sourcery function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
Examples from clojuredocs.org: [clojuredocs or cdoc]
          (user/clojuredocs name-here)
          (user/clojuredocs "ns-here" "name-here")
finance_manager.server=>

finance_manager.server=> (finance_manager.server/-main)
```

`(finance_manager.server/-main)` it will execute the -main function and
create a budget table in database.

Now open `src/finance_manager/models/db.clj` file and write the function
to add the budget record in the budget table.
```clojure
(defn add-budget [budget]
  (sql/with-connection
    db
    (sql/insert-record :budget budget)
  )
)
```

Now modify our `[:post "/addbudget"]` page to insert budget record.
```clojure

(defpage [:post "/addbudget"] budget
 (let
     (try
       (db/add-budget budget)
             (resp/redirect "/homepage")
             (catch Exception ex
               (render "/addbudget" (assoc budget :error (.getMessage ex)))
             )
     )
  )
)
```

Now create a page that will display list of all added budgets.

So first write a function in `src/finance_manager/models/db.clj` file to
get all records from database.
```clojure
  (defn db-read [query & args]
  (sql/with-connection
    db
    (sql/with-query-results
      res
      (vec (cons query args)) (doall res))))
```

```clojure
(defpage "/viewbudget" []
 (common/layout
    (let [budget (db/db-read "select * from budget")]
    (html
      [:h3 "Budget List Page"]
      [:table{:border="1"}
       [:tr
        [:th "Budget ID"]
        [:th "Budget Date"]
        [:th "Budget Amount"]
       ]
       (for [bgt budget]
        [:tr
          [:td (:id bgt)]
          [:td (:budget_date bgt)]
          [:td (:budget_amt bgt)]
         ]
       )]
     )
   )
  )
)
```

Now add links on homepage to add the budget and to view the budget.
```clojure
 (:use  hiccup.element)

 (defpage "/homepage" []
  (common/layout
    [:h3 "Finance Manager"]
    (html
      (link-to "/adbudget" "Add Budget")
      [:br]
      (link-to "/viewbudget" "View Budget")
    ) 
  )
)
```

#### You can also check for following :

* Tutorial 2 for :
   Form Validation.

* Tutorial 3 for :
   Session Management.
