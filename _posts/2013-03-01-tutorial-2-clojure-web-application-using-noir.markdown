---
layout: post
title: "Tutorial 2 - Clojure Web Application Using Noir"
date: 2013-03-01 10:53
comments: true
author: Deepali
categories: 
redirect_from: 
  - /2013/03/01/tutorial-2-clojure-web-application-using-noir/
  - /blog/2013/03/01/tutorial-2-clojure-web-application-using-noir/
---

In this tutorial we are going to cover how to apply form validations in
Noir.

Open `src/finance_manager/views/welcome.clj` file write following code
in it.

```
(:require [noir.validation :as vali])

(defn valid-budget? [{:keys [budget_date budget_amt]}]
  (vali/rule (vali/has-value? budget_date)
             [:budget_date "Budget Date Is Required."])
  (vali/rule (vali/has-value? budget_amt)
             [:budget_amt "Budget Amount Is Required."]) 
  (not (vali/errors? :budget_date :budget_amt)))
```
The above function will check that all the fields confirm to the rules,
such as budget_date and budget_amt are provided.

The rule have following form:

```
  (rule passed? [field error])
```

If the passed? condition is not met, add the error text to the given field.

We will need a helper for displaying the error on the page:

```
(defpartial error-item [[first-error]]
  [:p.error first-error]
)
```
 
Modify the `/addbudget` page as:

```
(defpage "/addbudget" {:keys [error]}
   (common/layout
     [:h2 "Add Monthly Budget"]
     [:div.error error]
     (form-to [:post "/addbudget"]
        (vali/on-error :budget_date error-item)

        (label "budget_date" "Select Budget Date")
        (text-field "budget_date")
        [:br]
        (vali/on-error :budget_amt error-item)

        (label "budget_amt" "Budget Amount")
        (text-field "budget_amt")
        [:br]
        (submit-button "Add Budget")
        (reset-button "Cancel")
     )
   )
)
```

<!-- more -->

Modify the `[:post "/addbudget"]` page as:

```
(defpage [:post "/addbudget"] budget
  (if (valid-budget? budget)
     (let
         (try
           (db/add-budget budget)
                 (resp/redirect "/homepage")
                 (catch Exception ex
                   (render "/addbudget" (assoc budget :error (.getMessage ex)))
                 )
         )
      )
       (render "/addbudget")
  )
)
```

If the budget is valid then it will add the record in the database. And
if the budget is not valid then it will render to `"/addbudget"` page
and displays respected errors on that page.
