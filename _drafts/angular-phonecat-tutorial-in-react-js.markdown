---
layout: post
title: "Angular PhoneCat Tutorial in React JS"
date: 2014-08-05 12:36:55 +0530
author: Girish
---

In this post, we are going to re-implement the Angular JS [PhoneCat tutorial](https://docs.angularjs.org/tutorial)
in React JS.

## About React JS
[React JS](http://facebook.github.io/react/) for the uninitiated, is a javascript framework from Facebook. Most commonly used as a V in MVC.
React uses Virtual DOM, an in-memory representation of the browser DOM, making it possible to re-render our entire app on every data change.
Just like the good old 90's!

Developing apps in React requires thinking differently than other [typical](https://angularjs.org/)
[javascript](http://backbonejs.org/) [frameworks](http://emberjs.com/).

### React JS Primer
#### No Templates but Components
Enough treating Views as a string. React uses javascript objects (called components) to represent the app markup.
Components facilitate separation of concerns and can be unit tested in isolation. We are not going to do any testing in this tutorial
(very bad, I know!).
Click [here](http://facebook.github.io/react/docs/test-utils.html) to read more about React testing.

React components are created with `React.createClass` method. Every component must implement a `render` method.

Here is a very simple `Hello` component.

```javascript
Hello = React.createClass({
  render: function(){
    return React.DOM.div(null, "Hello")
  }
})

React.renderComponent(Hello(), document.body);
```
As you can imagine, `React.renderComponent` renders our `Hello` component in `document.body`.

#### JSX
XML like HTML markup inside javascript is called JSX.
JSX makes it easy for non-developers (e.g. designers) to edit the components.
JSX is converted to javascript by the JSX transformer.
`/** @jsx React.DOM */` comment, signifies a `jsx` file, so JSX transformer can act on it.

For example

```javascript
/** @jsx React.DOM */
Hello = React.createClass({
  render: function(){
    return <div>Hello</div>
  }
})
```

gets converted to

```javascript
Hello = React.createClass({
  render: function(){
    return React.DOM.div(null, "Hello")
  }
})
```

Head over to [displaying-data](http://facebook.github.io/react/docs/displaying-data.html) for a more complete primer.

## PhoneCat Tutorial
PhoneCat is a catalog of Android devices, which you can filter, sort and see details of the device.

![alt text](/public/images/phonecat/catalog_screen.png)

Click [Final Demo] TODO to see what we are going build at the end of the tutorial.

## 0 Bootstrapping
[reactjs-phonecat](TODO) is git repo for this tutorial. Clone the repo and execute following commands in the project directory

```
git checkout -f step-0
npm install
bower install
```

Once successfully done, you can run the server by executing

```
gulp dev
```

Goto http://127.0.0.1:4000/ in your browser. You should see a success message.

## 1 Mockups
Let's start with the mockups of the actual pages we are going to create.

```
git checkout -f step-1
```
We have added html mockups, stylesheets and images. You can see the mockups here


[http://127.0.0.1:4000/mock_index.html](http://127.0.0.1:4000/mock_index.html)


[http://127.0.0.1:4000/mock_show.html](http://127.0.0.1:4000/mock_show.html)


### PhoneCat components
Our next task is identifying the components. This is a very subjective topic and each individual can have different opinions.
Here are the components I've decided for the Home page.

![alt text](/public/images/phonecat/home-page.png)

```
- PhoneCatWrapper (blue)
  - PhoneCat (yellow)
    - SearchForm (red)
    - PhonesList (orange)
      - Phone (green)
```


## 2 PhoneCatWrapper component
Let's modify `index.html`, and use it as layout for the app.

__src/index.html__

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>React Phone Gallery</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <link rel="stylesheet" href="assets/vendor/bootstrap.css" />
  <link rel="stylesheet" href="assets/main.css" />
</head>
<body>
  <div id="app" class="container">

  </div>

  <script type="text/javascript" src="assets/main.js"></script>
</body>
</html>
```

`main.js` requires the `PhoneCatWrapper.js` and renders it in `#id`

__src/scripts/main.js__

```html
/** @jsx React.DOM */

// Bring in jQuery and React as a Bower component in the global namespace
require("script!react/react-with-addons.js")
require("script!jquery/jquery.js")

var PhoneCatWrapper = require("./components/PhoneCatWrapper.js")

React.renderComponent(<PhoneCatWrapper />, document.getElementById('app'))
```

We have copied contents of `div.row` from `mock_index.html` in the render function of PhoneCatWrapper.


__src/scripts/components/PhoneCatWrapper.js__


```javascript
/** @jsx React.DOM */

var PhoneCatWrapper = React.createClass({
  render: function() {
    return (
      <div class="row">
        <div class="col-md-2">
          Search:
          <input type="text"/>
          Sort by:
          <select >
            <option value="name">Alphabetical</option>
            <option value="age">Newest</option>
          </select>
        </div>
        <div class="col-md-10">
          <ul class="phones">
          <li class="thumbnail phone-listing">
            <a href="#/phones/motorola-xoom-with-wi-fi" class="thumb"><img src="/images/phones/motorola-xoom-with-wi-fi.0.jpg"></a>
            <a href="#/phones/motorola-xoom-with-wi-fi" >Motorola XOOM™ with Wi-Fi</a>
            <p>The Next, Next Generation

              Experience the future with Motorola XOOM with Wi-Fi, the world's first tablet powered by Android 3.0 (Honeycomb).
            </p>
          </li>

          <!-- code deleted for brevity -->

        </ul>
        </div>
      </div>
    )
  }
});

module.exports = PhoneCatWrapper;
```
Right now if you refresh the browser, it fails saying `Line 19: Expected corresponding XJS closing tag for img`
JSX needs strict XML. Let's add the `img` closing tags.

Now it's working but the layout is all messed up.
TODO PREVIEW?
This is because JSX expects
HTML attributes in a slightly different manner, the culprit here is `class` it's
not showing up in the rendered HTML. To fix this replace all `class` attributes
with `className`.

Now it's working as expected!

## 3 Extract all static components
To speedup the things,
let's extract all the components from `PhoneCatWrapper`. Note that all these components
have static html.

You won't notice any difference in the UI, it's the same old html rendered in browser.
But we have modularized our code, more maintainable.

We intend to use `PhoneCatWrapper` as a thin wrapper having `state` around `PhoneCat` component.

__src/scripts/components/PhoneCatWrapper.js__

```javascript
var PhoneCat = require("./PhoneCat.js")

var PhoneCatWrapper = React.createClass({
  render: function() {
    return (
      <PhoneCat />
    )
  }
});
```

`PhoneCat` is composed of `SearchForm` and `PhonesList` components.

__src/scripts/components/PhoneCat.js__

```javascript
/** @jsx React.DOM */
var PhonesList = require("./PhonesList.js")
var SearchForm = require("./SearchForm.js")

var PhoneCat = React.createClass({
  render: function() {
    return (
      <div className="row">
        <div className="col-md-2">
          <SearchForm />
        </div>
        <div className="col-md-10">
          <PhonesList />
        </div>
      </div>
    )
  }
});

module.exports = PhoneCat;
```

`PhonesList` includes multiple `Phone` components.

__src/scripts/components/PhonesList.js__

```javascript
/** @jsx React.DOM */
var Phone = require("./Phone.js")

var PhonesList = React.createClass({
  render: function() {
    return (
      <ul className="phones">
        <Phone />
        <Phone />
      </ul>
    )
  }
});

module.exports = PhonesList;
```

`Phone` - an individual phone - has the static html markup.

__src/scripts/components/Phone.js__

```javascript
/** @jsx React.DOM */

var Phone = React.createClass({
  render: function() {
    return (
      <li className="thumbnail phone-listing">
        <a href="#/phones/motorola-xoom-with-wi-fi" className="thumb"><img src="/images/phones/motorola-xoom-with-wi-fi.0.jpg" /></a>
        <a href="#/phones/motorola-xoom-with-wi-fi" >Motorola XOOM™ with Wi-Fi</a>
        <p>The Next, Next Generation

          Experience the future with Motorola XOOM with Wi-Fi, the world's first tablet powered by Android 3.0 (Honeycomb).
        </p>
      </li>
    )
  }
});

module.exports = Phone;

```

Finally, the `SearchForm`.

__src/scripts/components/SearchForm.js__

```javascript
/** @jsx React.DOM */

var SearchForm = React.createClass({
  render: function() {
    return (
      <div>
        Search:
        <input type="text"/>
        Sort by:
        <select >
          <option value="name">Alphabetical</option>
          <option value="age">Newest</option>
        </select>
      </div>
    )
  }
});

module.exports = SearchForm;

```


## 4 Add dynamism
Enough of static components, let's make 'em dynamic!

Enter `props`. `props` (short for properties) is how components talk to each other.

Our API is going to return JSON that looks like `PHONES` array.

__src/scripts/components/PhoneCatWrapper.js__

{% highlight javascript linenos %}
/** @jsx React.DOM */
var PhoneCat = require("./PhoneCat.js")

var PHONES = [
    {
        "age": 0,
        "id": "motorola-xoom-with-wi-fi",
        "imageUrl": "/images/phones/motorola-xoom-with-wi-fi.0.jpg",
        "name": "Motorola XOOM\u2122 with Wi-Fi",
        "snippet": "The Next, Next Generation\r\n\r\nExperience the future with Motorola XOOM with Wi-Fi, the world's first tablet powered by Android 3.0 (Honeycomb)."
    },
    {
        "age": 1,
        "id": "motorola-xoom",
        "imageUrl": "/images/phones/motorola-xoom.0.jpg",
        "name": "MOTOROLA XOOM\u2122",
        "snippet": "The Next, Next Generation\n\nExperience the future with MOTOROLA XOOM, the world's first tablet powered by Android 3.0 (Honeycomb)."
    },
    {
        "age": 2,
        "carrier": "AT&T",
        "id": "motorola-atrix-4g",
        "imageUrl": "/images/phones/motorola-atrix-4g.0.jpg",
        "name": "MOTOROLA ATRIX\u2122 4G",
        "snippet": "MOTOROLA ATRIX 4G the world's most powerful smartphone."
    },
    {
        "age": 3,
        "id": "dell-streak-7",
        "imageUrl": "/images/phones/dell-streak-7.0.jpg",
        "name": "Dell Streak 7",
        "snippet": "Introducing Dell\u2122 Streak 7. Share photos, videos and movies together. It\u2019s small enough to carry around, big enough to gather around."
    }
]

var PhoneCatWrapper = React.createClass({
  render: function() {
    return (
      <PhoneCat phones={PHONES} />
    )
  }
});
{% endhighlight %}

On Line 39 - we are passing `PhoneCat` a property `phones`
and assigning it to the `PHONES` array. In JSX you need to include javascript
code inside the curly braces.

__src/scripts/components/PhoneCat.js__

```javascript
var PhoneCat = React.createClass({
  render: function() {
    return (
      <div className="row">
        <div className="col-md-2">
          <SearchForm />
        </div>
        <div className="col-md-10">
          <PhonesList phones={this.props.phones} />
        </div>
      </div>
    )
  }
});
```

In `PhoneCat` we are passing `phones` property to `PhonesList`.
You can access `props` inside the component as `this.props.property_name`

__src/scripts/components/PhonesList.js__

```javascript
var PhonesList = React.createClass({
  render: function() {
    var phones = this.props.phones.map(function(phone, i){
      return <Phone phone={phone} key={i} />
    });

    return (
      <ul className="phones">
        {phones}
      </ul>
    )
  }
});
```

In `PhonesList` `render` function, we are creating an array `phones` of `Phone` components
using the javascript [map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) function.
Then we are rendering that array. This is how you typically deal with a collection
of components in React.

[key](http://facebook.github.io/react/docs/multiple-components.html#dynamic-children) property
is used to uniquely identify the child components.


__src/scripts/components/Phone.js__

```javascript
var Phone = React.createClass({
  render: function() {
    var phone = this.props.phone;

    return (
      <li className="thumbnail phone-listing">
        <a href="#" className="thumb"><img src={phone.imageUrl} /></a>
        <a href="#">{phone.name}</a>
        <p>{phone.snippet}</p>
      </li>
    )
  }
});
```

In `Phone` we are using the `phone` property to assign the image, name and snippet.
Let's worry about the links when we deal with the `PhoneDetails` component.

## 5 Filter and Sort
Now that we have a dynamically generated app. Let's add filter and sort features.

This is the time to identify the app state. Go through [this](http://facebook.github.io/react/docs/thinking-in-react.html#step-3-identify-the-minimal-but-complete-representation-of-ui-state)
article to read more about identifying the app state.
The only actions user can perform are filter and sort. So our state is
* filter text that user has entered
* sort order that user has selected

Next we need to identify where the app state should live. Go through [this](http://facebook.github.io/react/docs/thinking-in-react.html#step-4-identify-where-your-state-should-live)
article read more about identifying where the state should live.
In our app, we are going hold the state in `PhoneCat` component.


__src/scripts/components/PhoneCat.js__

```javascript
var PhoneCat = React.createClass({
  getInitialState: function() {
      return {
        filter_text: '',
        sort_by: 'age'
      }
  },

  handleSearch: function(filter_text, sort_by) {
    this.setState({
      filter_text: filter_text,
      sort_by: sort_by
    })
  },

  render: function() {
    return (
      <div className="row">
        <div className="col-md-2">
          <SearchForm searchHandler={this.handleSearch} filter_text={this.state.filter_text} sort_by={this.state.sort_by} />
        </div>
        <div className="col-md-10">
          <PhonesList phones={this.props.phones} filter_text={this.state.filter_text} sort_by={this.state.sort_by} />
        </div>
      </div>
    )
  }
});
```

We need to implement the `getInitialState` function,
to return `{ filter_text: '', sort_by: 'age' }`, this is initial state our
app should have.

Next we have added `handleSearch` function, it sets the state with `this.setState`.
Whenever `this.setState` is executed React will automatically re-render our app
with the minimal set of DOM changes required.

Then we are passing `filter_text` and `sort_by` to `SearchForm` and `PhonesList`
as props. We are also passing reference to `this.handleSearch` as a prop to `SearchForm`.
This is the recommended way if you need invoke a parent component method from a child component.

__src/scripts/components/SearchForm.js__

```javascript
var SearchForm = React.createClass({
  onChangeHandler: function() {
    var query = this.refs.query.getDOMNode().value.trim();
    var order = this.refs.order.getDOMNode().value;
    this.props.searchHandler(query, order);
  },

  render: function() {
    return (
      <div>
        Search:
        <input type="text" value={this.props.filter_text} ref="query" onChange={this.onChangeHandler} />
        Sort by:
        <select value={this.props.sort_by} ref="order" onChange={this.onChangeHandler}>
          <option value="name">Alphabetical</option>
          <option value="age">Newest</option>
        </select>
      </div>
    )
  }
});
```
In `SearchForm` we are using props `filter_text` and `sort_by` set values of
`input` and `select`.

For handling user events, React provides a cross-browser synthetic event system.
For handling `input` and `select` change events, React provides `onChange` event.
To read more about synthetic events go [here](http://facebook.github.io/react/docs/events.html).

`onChangeHandler` function uses `refs` to get hold of the form elements.
`refs` is a special property, to read more go [here](http://facebook.github.io/react/docs/more-about-refs.html#the-ref-attribute).

`getDOMNode()` returns the native DOM object as you can imagine.

`onChangeHandler` then invokes `this.props.searchHandler(query, order)`, which
is a reference to `handleSearch` function from `PhoneCat`.

__src/scripts/components/PhonesList.js__

```javascript
var PhonesList = React.createClass({

  render: function() {
    var props = this.props;

    var filtered = $.grep(this.props.phones, function(phone) {
      return phone.name.toLowerCase().indexOf(props.filter_text) > -1;
    });

    var sorted = filtered.sort(function(a, b) {
        if(props.sort_by === 'name')
          return a.name.localeCompare(b.name)
        else
          return a.age - b.age
      });

    var phones = sorted.map(function(phone, i){
      return <Phone phone={phone} key={i} />
    });

    return (
      <ul className="phones">
        {phones}
      </ul>
    )
  }
});
```

The `render` function of `PhonesList`, filters the `phones` property using
[jQuery.grep](http://api.jquery.com/jquery.grep/). Then sorts the filtered
phones using javascript [sort](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/sort) function.

## 6 Ajax
In this step we will get the data from our JSON API. There is no API for the tutorial, we will
just fetch `/phones/phones.json` from the server.

Let's get rid of the static `PHONES` array.

__src/scripts/components/PhoneCatWrapper.js__

```javascript
var PhoneCatWrapper = React.createClass({
  getInitialState: function() {
      return {
        phones: []
      }
  },

  componentDidMount: function() {
    $.getJSON('/phones/phones.json', (function(data) {
      this.setState({phones: data});
    }).bind(this));

  },

  render: function() {
    return (
      <PhoneCat phones={this.state.phones} />
    )
  }
});
```

We are going to store the phones data returned from the API in
`PhoneCatWrapper` state.
`getInitialState` function returns an empty `phones` array.

The `componentDidMount` function is invoked immediately after the component is rendered
in the browser DOM.
This is a good place to call our API using [jQuery.getJSON](http://api.jquery.com/jquery.getjson/) function.
On success we are storing the data in state using `this.setState`.

### We will implement the phone details page in next part of this post.
