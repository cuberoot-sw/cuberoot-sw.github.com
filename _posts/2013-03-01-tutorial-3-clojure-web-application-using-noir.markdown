---
layout: post
title: "Tutorial 3 - Clojure Web Application Using Noir"
date: 2013-03-01 11:25
comments: true
author: Deepali
categories: 
---

In this tutorial we are going to cover how to manage a Session in
Noir.

We need to provide the facility to users to login with their accounts.

So we have to first create pages for login and signup in our
`src/finance_manager/views/welcome.clj` file. Also we have to create a
table for users in our `finance_manager_db` database to store users
record.

First create table for `users` in `src/finance-manager/models/db.clj`

```clojure
(defn create-user-table []
  (sql/with-connection
      db
      (sql/create-table
        :users
        [:id "SERIAL"]
        [:username "varchar(100)"]
        [:password "varchar(100)"]
       )
    )
)

```
 
Also write function to insert users record in users table. And a
function to fetch user record from users table.

```clojure
(defn add-user [user]
  (sql/with-connection
    db
    (sql/insert-record :users user)
  )
)

(defn get-user [username]
  (first
  (db-read "select * from users where username=?" username))
)

```
<!-- more -->
Open `src/finance_manager/views/welcome.clj` file and create pages for
login and signup.

```clojure
(:require [noir.util.crypt :as crypt])

 ;;Login Page

(defpage "/" {:keys [error]}
  (common/layout
   [:div.error error]
   [:h2 "Login Page"]
   (form-to [:post "/login"]
     (label "username" "Username")
     (text-field "username")
      [:br]
     (label "password" "Password")
     (password-field "password")
     [:br]
     (submit-button {:class "btn"} "Login")
     (reset-button {:class "btn"} "Cancel")
   )
  )
)

;; Login Page Handler

(defpage [:post "/login"] user
  (let [getuser (db/get-user (:username user))]
          (if (and getuser (crypt/compare (:password user) (:password getuser)))
              (render "/homepage" (session/get :user))
              (render "/" (assoc user :error "Login Failed"))
          )
  )
)


;;Signup Page

(defpage "/signup" {:keys [error]}
  (common/layout
    [:h3 "Signup Form"]
    [:div.error error]
    (form-to [:post "/signup"]
      (label "username" "Username")
      (text-field "username")
      [:br]
      (label "password" "Password")
      (password-field "password")
      [:br]
      (submit-button "Signup")
      (reset-button "Cancel")
    )
  )
)

;; Signup Page Handler

(defpage [:post "/signup"] user
    (try
      (db/add-user (update-in user[:password] crypt/encrypt))
      (render "/homepage")
      (catch Exception ex
        (render "/signup" (assoc user :error (.getMessage ex)))
      )
    )
)
```

`crypt/encrypt` is used to encrypt the given password.

`crypt/compare` is used to compare the raw string with already encrypted
string.

For managing the session open `src/finance_manager/views/welcome.clj`
file.

```clojure
(:require [noir.session :as session])
```

Then modify our `[:post "/login"]` and `[:post "/signup"]` pages.

```clojure
(defpage [:post "/login"] user
     (let [getuser (db/get-user (:username user))]
              (if (and getuser (crypt/compare (:password user) (:password getuser)))
                (do
                   (session/put! :user (:username user))
                   (render "/homepage" (session/get :user))
                )
                 (render "/" (assoc user :error "Login Failed"))
              )
       )
)

(defpage [:post "/signup"] user
    (try
      (db/add-user (update-in user[:password] crypt/encrypt))
      (session/put! :user (:username user))
      (render "/homepage" (session/get! :user))
      (catch Exception ex
        (render "/signup" (assoc user :error (.getMessage ex)))
      )
    )
)
```

`session/put!` Associates the key with the given value in the session.

`session/get` Get the key's value from the session, returns nil if it doesn't exist.

Now create a page for logout to clear the session in
`src/finance_manager/views/welcome.clj` file.

```clojure
 (defpage "/logout" []
  (session/clear!)
  (resp/redirect "/")) 
```

`session/clear!` Remove all data from the session.

