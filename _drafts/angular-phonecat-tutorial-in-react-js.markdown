---
layout: post
title: "Angular PhoneCat Tutorial in React JS"
date: 2014-08-08 12:36:55 +0530
author: Girish
---

In this tutorial, we are going to re-implement the Angular JS [PhoneCat tutorial](https://docs.angularjs.org/tutorial)
in React JS.


{% include tutorial_headers.html step="12" demo_text="Live Final Demo" %}


## PhoneCat Tutorial
PhoneCat is a catalog of Android devices, which you can filter, sort and see details of the device.

![alt text](/public/images/phonecat/catalog_screen.png)

In this __12 step__ tutorial you will learn about:

1.  React JS
2.  Creating components
3.  Components interaction
4.  Routing in React

After you complete all the steps in this tutorial, you’ll have an app that looks something like this:

{% include tutorial_headers.html step="12" demo_text="Live Final Demo" %}



## About React JS
[React JS](http://facebook.github.io/react/) for the uninitiated, is a javascript framework from Facebook/Instagram. Most commonly used as a V in MVC.
React is used in production by FaceBook, Instagram, Khanacademy, Atom editor and many more.

React uses Virtual DOM, an in-memory representation of the browser DOM, making it possible to re-render our entire app on every data change.
Just like the good old 90's!

Developing apps in React requires different thinking than other [typical](https://angularjs.org/)
[javascript](http://backbonejs.org/) [frameworks](http://emberjs.com/).


<!-- more -->

### React JS Primer
#### Components Not Templates
Enough treating Views as strings. React uses javascript objects (called components) to represent the app markup.
Components facilitate separation of concerns and can be unit tested in isolation. We are not doing any testing in this tutorial
(very bad, I know!).
Click [here](http://facebook.github.io/react/docs/test-utils.html) are interested in React testing.

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
JSX makes it easy for non-developers (for example designers) to edit the components.
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







## Step-0 Bootstrapping

{% include tutorial_headers.html step=0 prev_step="0~2" %}

Tutorial sources are available at [reactjs-phonecat](https://github.com/girishso/reactjs-phonecat).

We are using [react-starter-template](https://github.com/johnthethird/react-starter-template) for the tutorial.
[node.js](http://nodejs.org/) is used as local server
and [gulp.js](http://gulpjs.com/) as build tool.

To get strated, clone the repo and execute following commands in the project directory

```
$ git clone https://github.com/girishso/reactjs-phonecat#
$ cd reactjs-phonecat
$ git checkout -f step-0
$ npm install
$ bower install
```

Once successfully done, you can run the server by executing

```
$ gulp dev
```

Goto http://127.0.0.1:4000/ in your browser. You should see a success message.






## Step-1 Mockups

{% include tutorial_headers.html step="1" prev_step="0" demo_page="mock_index.html" demo_text="Mock home page" %}
{% include tutorial_headers.html step="1" demo_page="mock_show.html" demo_text="Mock phone page" %}

Let's start with the mockups of the actual pages we are going to create.

{% include tutorial_setup.html step="1" %}

We have added html mockups, stylesheets and images. You can see the mockups here


http://127.0.0.1:4000/mock_index.html


http://127.0.0.1:4000/mock_show.html


### PhoneCat components
Our next task is identifying the components. This is a very subjective topic and everybody can have different opinions.
Here are the components I've decided to go with for the Home page.

![alt text](/public/images/phonecat/home-page.png)

Here is our components hierarchy -

```
- PhoneCatWrapper (blue)
  - PhoneCat (yellow)
    - SearchForm (red)
    - PhonesList (orange)
      - Phone (green)
```






## Step-2 PhoneCatWrapper component

{% include tutorial_headers.html step="2" prev_step="1" %}

{% include tutorial_setup.html step="2" %}

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

`main.js` is the entry point of our app. It renders the `PhoneCatWrapper` component in `#id`

__src/scripts/main.js__

```html
/** @jsx React.DOM */
require("script!react/react-with-addons.js")
require("script!jquery/jquery.js")
var PhoneCatWrapper = require("./components/PhoneCatWrapper.js")
React.renderComponent(<PhoneCatWrapper />, document.getElementById('app'))
```

We have copied contents of `div.row` from `mock_index.html` in the render function of `PhoneCatWrapper`.


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
            <a href="#/phones/motorola-xoom-with-wi-fi" class="thumb"><img src="images/phones/motorola-xoom-with-wi-fi.0.jpg"></a>
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
Right now if you refresh the browser, it fails saying `Line 19: Expected corresponding XJS closing tag for img`.
JSX needs strict XML. Let's add the `img` closing tags.

Now it's working but the layout is all messed up.
This is because JSX expects
HTML attributes in a slightly different manner, the culprit here is `class` it's
not showing up in the rendered HTML. To fix this replace all `class` attributes
with `className`.

Now it's working as expected!






## Step-3 Extract all static components

{% include tutorial_headers.html step="3" prev_step="2" %}

{% include tutorial_setup.html step="3" %}

To speedup the things,
let's extract all the remaining components from `PhoneCatWrapper`. Note that all these components
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

`Phone` represents... well a phone!

__src/scripts/components/Phone.js__

```javascript
/** @jsx React.DOM */

var Phone = React.createClass({
  render: function() {
    return (
      <li className="thumbnail phone-listing">
        <a href="#/phones/motorola-xoom-with-wi-fi" className="thumb"><img src="images/phones/motorola-xoom-with-wi-fi.0.jpg" /></a>
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




## Setp-4 Add dynamism

{% include tutorial_headers.html step="4" prev_step="3" %}

{% include tutorial_setup.html step="4" %}

Enough of static components, let's make 'em dynamic!

Enter `props` (short for properties), `props` is how components talk to each other.

Our API is going to return JSON that looks like `PHONES` array.

__src/scripts/components/PhoneCatWrapper.js__

```javascript
/** @jsx React.DOM */
var PhoneCat = require("./PhoneCat.js")

var PHONES = [
    {
        "age": 0,
        "id": "motorola-xoom-with-wi-fi",
        "imageUrl": "images/phones/motorola-xoom-with-wi-fi.0.jpg",
        "name": "Motorola XOOM\u2122 with Wi-Fi",
        "snippet": "The Next, Next Generation\r\n\r\nExperience the future with Motorola XOOM with Wi-Fi, the world's first tablet powered by Android 3.0 (Honeycomb)."
    },
    {
        "age": 1,
        "id": "motorola-xoom",
        "imageUrl": "images/phones/motorola-xoom.0.jpg",
        "name": "MOTOROLA XOOM\u2122",
        "snippet": "The Next, Next Generation\n\nExperience the future with MOTOROLA XOOM, the world's first tablet powered by Android 3.0 (Honeycomb)."
    },
    {
        "age": 2,
        "carrier": "AT&T",
        "id": "motorola-atrix-4g",
        "imageUrl": "images/phones/motorola-atrix-4g.0.jpg",
        "name": "MOTOROLA ATRIX\u2122 4G",
        "snippet": "MOTOROLA ATRIX 4G the world's most powerful smartphone."
    },
    {
        "age": 3,
        "id": "dell-streak-7",
        "imageUrl": "images/phones/dell-streak-7.0.jpg",
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
```


We are passing `PhoneCat` a property `phones`
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







## Step-5 Filter and Sort

{% include tutorial_headers.html step="5" prev_step="4" %}

{% include tutorial_setup.html step="5" %}

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






## Step-6 Ajax

{% include tutorial_headers.html step="6" prev_step="5" %}

{% include tutorial_setup.html step="6" %}

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




## Step-7 Phone page

{% include tutorial_headers.html step="7" prev_step="6" %}

{% include tutorial_setup.html step="7" %}

Now that we are done with the home page, it's time to add the Phone page. Here is the screenshot of our mock page.

![alt text](/public/images/phonecat/phone-page.png)

Here is our components hierarchy for the phone page -

```
- PhoneDetailsWrapper (blue)
  - PhoneDetails (green)
    - ImageGallery (red)
```
We will display Phone page on path `/phones/some-phone-id`.

Before we get into creating Phone page components, we need to resolve one issue. How is our app
going to load the Phone page for path `/phones/some-phone-id`?

We need some routing framework for this, remember React only deals with the V of the MVC.
There are many possibilities - we could use [backbone](http://backbonetutorials.com/what-is-a-router/) routes,
[crossroads](http://millermedeiros.github.io/crossroads.js/) or many more.

We are going with [react-router](https://github.com/rackt/react-router), it's specific to React and
will be easier to deal with.

__src/scripts/main.js__

```javascript
require('script!react-router/dist/react-router.js')

var Route = ReactRouter.Route;
var Link = ReactRouter.Link;
var PhoneCatWrapper = require("./components/PhoneCatWrapper.js")
var PhoneDetailsWrapper = require("./components/PhoneDetailsWrapper.js")

App = React.createClass({
  render: function(){
    return(
      <div>
        <ul>
          <li><Link to="phones">Home</Link></li>
        </ul>
        <this.props.activeRouteHandler/>
      </div>
    )
  }
})

React.renderComponent(
  (<Route handler={App}>
      <Route name="phones" handler={PhoneCatWrapper}/>
      <Route name="phone" path="/phones/:phoneId" handler={PhoneDetailsWrapper} />
    </Route>), document.getElementById('app'));
```

Note: Home page will be accessible on - http://127.0.0.1:4000/#/phones

Here we have created an `App` component, that serves as routes handler. In the `render` function
we are rendering a very basic nav-bar with only Home link. `<this.props.activeRouteHandler/>` renders the
active route handler. You can read more about activeRouteHandler [here](https://github.com/rackt/react-router/blob/master/docs/guides/overview.md).

Then in `React.renderComponent`, we are defining our routes. Route handler is `App`, `phones` route handler is `PhoneCatWrapper`
and `phone` route handler is `PhoneDetailsWrapper`. `path="/phones/:phoneId"` makes the `phoneId` accessible to the
handler via props as we will see later.

Now let's add `PhoneDetailsWrapper` component, copy and paste `section#main` from the `mock_show.html` page to `render` function. Don't forget to
add `<img>` closing tags and change `class` to `className`!

__src/scripts/components/PhoneDetailsWrapper.js__

```javascript
var PhoneDetailsWrapper = React.createClass({
  render: function() {
    return (
      <section id="main"><div className="phone-images"><img src="images/phones/motorola-xoom-with-wi-fi.3.jpg" className="phone" />
      </div>
      <h1>Motorola XOOM™ with Wi-Fi</h1>
      <p>Motorola XOOM with Wi-Fi has a super-powerful dual-core processor and Android™ 3.0 (Honeycomb) — the Android platform designed specifically for tablets. With its 10.1-inch HD widescreen display, you’ll enjoy HD video in a thin, light, powerful and upgradeable tablet.</p>
      <ul className="phone-thumbs"><li><img src="images/phones/motorola-xoom-with-wi-fi.0.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.1.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.2.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.3.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.4.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.5.jpg" />
      </li></ul>
      <ul className="specs"><li>
          <span>Availability and Networks</span>
          <dl>
              <dt>Availability</dt>
              <dd></dd>
          </dl>
      </li>

      <li>
          <span>Battery</span>
          <dl>
              <dt>Type</dt>
              <dd>Other ( mAH)</dd>
              <dt>Talk Time</dt>
              <dd>24 hours</dd>
              <dt>Standby time (max)</dt>
              <dd>336 hours</dd>
          </dl>
      </li>
      <!-- code deleted for brevity -->
      </ul>
      </section>
    )
  }
});

module.exports = PhoneDetailsWrapper;
```

__src/scripts/components/Phone.js__

```javascript
Link = ReactRouter.Link;

var Phone = React.createClass({
  render: function() {
    var phone = this.props.phone;

    return (
      <li className="thumbnail phone-listing">
        <Link to="phone" phoneId={phone.id} className="thumb" >
          <img src={phone.imageUrl} />
        </Link>
        <Link to="phone" phoneId={phone.id} >
          {phone.name}
        </Link>
        <p>{phone.snippet}</p>
      </li>
    )
  }
});
```
We have modified `Phone` component to include `Link` to the Phone details page.
So the links on Home page should finally be working!



## Step-8 PhoneDetails and ImageGallery components

{% include tutorial_headers.html step="8" prev_step="7" %}

{% include tutorial_setup.html step="8" %}

Now let's extract `PhoneDetails` component from `PhoneDetailsWrapper`. We have simply
moved the `render` method from `PhoneDetailsWrapper` to `PhoneDetails`.

__src/scripts/components/PhoneDetailsWrapper.js__

```javascript
var PhoneDetails = require("./PhoneDetails.js")

var PhoneDetailsWrapper = React.createClass({
  render: function() {
    return (
      <PhoneDetails />
    )
  }
});
```

__src/scripts/components/PhoneDetails.js__

```javascript
var PhoneDetails = React.createClass({
  render: function() {
    return (
      <section id="main"><div className="phone-images"><img src="images/phones/motorola-xoom-with-wi-fi.3.jpg" className="phone" />
      </div>
      <h1>Motorola XOOM™ with Wi-Fi</h1>
      <p>Motorola XOOM with Wi-Fi has a super-powerful dual-core processor and Android™ 3.0 (Honeycomb) — the Android platform designed specifically for tablets. With its 10.1-inch HD widescreen display, you’ll enjoy HD video in a thin, light, powerful and upgradeable tablet.</p>

      <ImageGallery />

      <ul className="specs"><li>
          <span>Availability and Networks</span>
          <dl>
              <dt>Availability</dt>
              <dd></dd>
          </dl>
      </li>

      <li>
          <span>Battery</span>
          <dl>
              <dt>Type</dt>
              <dd>Other ( mAH)</dd>
              <dt>Talk Time</dt>
              <dd>24 hours</dd>
              <dt>Standby time (max)</dt>
              <dd>336 hours</dd>
          </dl>
      </li>
      <!-- code deleted for brevity -->
      </section>
    )
  }
});

module.exports = PhoneDetails;
```

Let's extract `ImageGallery` component from `PhoneDetails`.

__src/scripts/components/ImageGallery.js__

```javascript
var ImageGallery = React.createClass({
  render: function() {
    return (
      <ul className="phone-thumbs"><li><img src="images/phones/motorola-xoom-with-wi-fi.0.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.1.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.2.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.3.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.4.jpg" />
      </li><li><img src="images/phones/motorola-xoom-with-wi-fi.5.jpg" />
      </li></ul>
    )
  }
});

module.exports = ImageGallery;
```




## Step-9 Get phone details on Ajax

{% include tutorial_headers.html step="9" prev_step="8" %}

{% include tutorial_setup.html step="9" %}

To save us time, instead of using a static `PHONE` array in `PhoneDetailsWrapper`, we are
directly loading it using Ajax from the server.

__src/scripts/components/PhoneDetailsWrapper.js__

```javascript
var PhoneDetailsWrapper = React.createClass({
  getInitialState: function() {
    return {
      phone: {
        images: [],
        battery: {},
        storage: {},
        connectivity: {},
        android: {},
        sizeAndWeight: {
          dimensions: []
        },
        display: {},
        hardware: {},
        camera: {
          features: []
        }
      }
    };
  },
  componentDidMount: function() {
    var url;
    url = "phones/" + this.props.params.phoneId + ".json";
    return $.getJSON(url, (function(data) {
      data.phone_image = data.images[0];
      return this.setState({
        phone: data
      });
    }).bind(this));
  },
  render: function() {
    return <PhoneDetails phone={this.state.phone} />
  }
});
```

We are saving phone details in `PhoneDetailsWrapper` state. `getInitialState` function returns the initial state required
to render the component.

In `componentDidMount` function we are making the Ajax call using jQuery. rack-router passes url parameters to the component in `this.props.params`
property, we are using `this.props.params.phoneId` to create the url for the Ajax call. On success we are setting the state with `this.setState`.

Finally, passing the state to `PhoneDetails` as `phone` property.





## Step-10 Use props in PhoneDetails

{% include tutorial_headers.html step="10" prev_step="9" %}

{% include tutorial_setup.html step="10" %}

Time to make PhoneDetails dynamic! Let's use the `phone` property.

__src/scripts/components/PhoneDetails.js__

```javascript
var PhoneDetails = React.createClass({
  checkmark: function(truthy) {
    if (truthy) {
      return String.fromCharCode(10003);
    } else {
      return String.fromCharCode(10008);
    }
  },

  render: function() {
    var phone = this.props.phone;
    var dimensions = phone.sizeAndWeight.dimensions.map(function(dimension, i) {
      return <dd key={i}>{ dimension }</dd>;
    });

    return (
      <section id="main">
        <div className="phone-images">
          <img ref="phone_image" src={ this.props.phone.images[0]} className="phone" />
        </div>
        <h1>{phone.name}</h1>
        <p>{phone.description}</p>

        <ImageGallery />

        <ul className="specs">
          <li>
              <span>Availability and Networks</span>
              <dl>
                  <dt>Availability</dt>
                  <dd>{ phone.availability }</dd>
              </dl>
          </li>
          <!-- code deleted for brevity -->
          <li>
              <span>Connectivity</span>
              <dl>
                  <dt>Network Support</dt>
                  <dd>{ phone.connectivity.cell }</dd>
                  <dt>WiFi</dt>
                  <dd>{ phone.connectivity.wifi }</dd>
                  <dt>Bluetooth</dt>
                  <dd>{ phone.connectivity.bluetooth }</dd>
                  <dt>Infrared</dt>
                  <dd>{ this.checkmark(phone.connectivity.infrared) }</dd>
                  <dt>GPS</dt>
                  <dd>{ this.checkmark(phone.connectivity.gps) }</dd>
              </dl>
          </li>
          <!-- code deleted for brevity -->
          <li>
              <span>Size and Weight</span>
              <dl>
                  <dt>Dimensions</dt>
                    { dimensions }
                  <dt>Weight</dt>
                  <dd>{ phone.sizeAndWeight.weight }</dd>
              </dl>
          </li>
          <!-- code deleted for brevity -->
      </section>
    );
  }
});
```
There is nothing new here, except the `checkmark` function. JSX has [issues](http://facebook.github.io/react/docs/jsx-gotchas.html)
displaying HTML entities. There are various workarounds, we are going ahead with using `String.fromCharCode`. The `checkmark` function simply
returns &#x2713; if the parameter `truthy` is true otherwise &#10008;.

You should see phone details dynamically populated.




## Step-11 Use props in ImageGallery

{% include tutorial_headers.html step="11" prev_step="10" %}

{% include tutorial_setup.html step="11" %}

Phone images are still not showing up in the gallery, let's do something about it!

__src/scripts/components/ImageGallery.js__

```javascript
var ImageGallery = React.createClass({
  render: function() {
    var images = this.props.images.map(function(image_path, i) {
      return <li key={i}><img src={image_path} /> </li>;
    });

    return (
      <ul className="phone-thumbs">
        {images}
      </ul>
    );
  }
});

module.exports = ImageGallery;
```
Nothing ground-breaking here, we are simply creating an array of images and rendering it.

Don't forget to pass the `images` property to `ImageGallery` in `PhoneDetails`!


__src/scripts/components/PhoneDetails.js__

```javascript
.
.
        <ImageGallery images={phone.images} />
.
.
```


Now you should see the dynamic thumbnails.



## Step-12 Make thumbnail click work

{% include tutorial_headers.html step="12" prev_step="11" %}

{% include tutorial_setup.html step="12" %}

Now we have everything in place, except thumbnail click is not working.
Time to decide what should go in the state... the only thing user can do is
click the thumbnail to change main image of the phone. We will save
`active_image` url in the state and change it on thumbnail click.


__src/scripts/components/PhoneDetails.js__

```javascript
var PhoneDetails = React.createClass({

  getInitialState: function() {
    return {
      active_image: ''
    }
  },
  .
  .
  .

  handleThumbClick: function(image_path) {
    this.setState( {
        active_image: image_path
      }
    )
  },

  render: function() {
    var phone = this.props.phone;
    var dimensions = phone.sizeAndWeight.dimensions.map(function(dimension, i) {
      return <dd key={i}>{ dimension }</dd>;
    });

    return (
      <section id="main">
        .
        .
        <ImageGallery images={phone.images} handleThumbClick={this.handleThumbClick} />
        .
        .
      </section>
    );
  }
});
```
The `handleThumbClick` function sets `active_image` state, and we are passing it's reference to the `ImageGallery` as a property.
So it can tell when/which thumbnail is clicked.


__src/scripts/components/ImageGallery.js__

```javascript
var ImageGallery = React.createClass({
  handleClick: function(e) {
    return this.props.handleThumbClick(e.target.src);
  },

  render: function() {
    var images = this.props.images.map(function(image_path, i) {
      return <li key={i}><img src={image_path} /> </li>;
    });

    return (
      <ul className="phone-thumbs" onClick={this.handleClick}>
        {images}
      </ul>
    );
  }
});
```

In `ImageGallery` we wrote an `onClick` handler `handleClick`.
The `handleClick` function, invokes `this.props.handleThumbClick` function
which points to `PhoneDetails` function `handleThumbClick`.

Now you should see the fully functional app in action!

That's it! We are finally done!! Please let me know your feedback/suggestions/critics in comments
or mail me at [girish@cuberoot.in](mailto:girish@cuberoot.in)
