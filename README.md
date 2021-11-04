# Introduction to React Router

## Objectives

1. How client-side routing works
2. What the trade-offs are for client-side routing
3. What `pushState` is

### Client-Side Routing

So, we have learned about building components, changing state, passing props,
etc... You may be wondering how you can make an app with multiple URLs that
contain different components. Not every app is a todo list, tic-tac-toe or a
spreadsheet. So how do we build an app that allows us to have unique pages for
the user to interact with? This is where `Client-Side` routing comes in.

**Client-Side** routing is a different beast than what we are used to with
traditional server side routing that comes with **Rails**, **Sinatra**, or
**Node/Express**, because we aren't making constant **HTTP GET** requests.

Lets say that our **Client-Side** app is going to have these routes

- `https://www.movie-maker-2016/movies/new`
- `https://www.movie-maker-2016/movies`
- `https://www.movie-maker-2016/about`
- `https://www.movie-maker-2016/login`

> **Note**: these links are examples and do not lead to any website

Our `server`'s only job is to render the `HTML`. Which will look similar to
this.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Movie Maker 2016</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  </head>
  <body>
    <div id="container"></div>
    <script src="./bundle.js" charset="utf-8"></script>
  </body>
</html>
```

With **Client-Side** routing, it is now the responsibility of the
**Client-Side-Code**, rather than the server, to handle the routing, fetching
and displaying of the data in the browser.

Imagine you've built a personal blog with a navigation that links your home
page, about page and contact page. With Client-Side routing, you might get all
the needed data to render all three pages on the first page load. Then, when a
user clicks around your site, the Client-Side router swaps the 'home page'
component with the 'about page' component and renders faster than it would if
you were requesting a separate page from a server.

Client-Side routing brings with it some great benefits. The major one is
_Speed_. Since we are only making one request to the server we don't have to
wait for a round trip server call for each page change. We have everything
stored on the Client-Side already, so we just notify our Client-Side code to
display the info as we need it.

### Single Page App (SPA)

In **React** we will likely be building an **SPA**, or Single Page Application.
This means we won't require multiple pages to be loaded from the server, just
the original **GET** request with our initial HTML, CSS and JS files. This
requires us to figure out how to make the experience of Client-Side routing work
to our advantage.

There are a couple of things that we need to take into consideration:

- We want to make sure that we have a URL that displays what the user is doing
  at that moment. So if they are viewing a bio page it might look like this
  `https://worlds-best-app/bio` instead of this `https://worlds-best-app`.

- We want a user to be able to use the browser's back and forward buttons with
  ease.

- We want a user to be able to input a URL into the address bar and navigate to
  the view they need to see.

This was easy with server side rendering: most MVC frameworks come with this for
free, because we just defined the routes, added the actions needed to the
controller and then made a call to the model to get the info we desired.

### Limits of Client-Side routing

So this all sounds great, but what are the limitations?

- Loading of CSS & Javascript

  Since we are now loading all of our CSS and Javascript on the initial **GET**
  request it can take a while to load our first page. This can be important as
  the first page load can take a long time if you have a huge application.

- Analytics

  Analytic tools normally track page views, but an SPA doesn't have pages in the
  traditional sense, so this makes it harder for Analytical tools to track page
  views. We will need to add extra scripts to handle this limitation.

- They are much harder to design.

We have to plan out all the possibilities that might happen on the
**Client-Side**; this might feel like we are repeating designs that we have
already completed with our server routes and models.

#### Push it, Push it

When we make server calls we are making a **GET** request to a URL and that new
URL is in our address bar. If we have visited a few different URL's that
information is saved in browser history.

Go to the JavaScript console in Chrome and type

```js
window.history;
// => History { length: 32, state: null, scrollRestoration: "auto" };
```

The length is how many locations you have visited in this window session.

Now if you type the following code it will take you to the last location in your
browser history.

```js
window.history.back();
```

Go ahead and try it out.

.............

............

..........

Oh good, you're back!! :)

![no you didn't!](http://i.giphy.com/10VbdHyZElXqso.gif)

So that is the JavaScript to emulate the experience of using the back button in
the browser toolbar. You can also move forward using
**window.history.forward()**.

With the JavaScript's History API we also have the ability to **pushState()** to
the history entries. This method takes in three parameters: **pushState(state,
title, url)**

- `state`: This is a plain JavaScript object that is associated with the new history
  entry we are going to create with the **pushState()** function.

- `title`: This is currently ignored by most browsers and it is safe to just pass an
  empty string or a title here.

- `url`: This is the URL for the new history entry. The browser will not attempt to
  load this URL after it calls pushState().

Why don't we go ahead and create a new url in our browser

```js
const newState = {
  goal: "Learn about pushState()",
};

window.history.pushState(newState, "new state", "new-state");
```

You should notice that your browser has now changed to show `new-state` at the
end of your URL address.

Go ahead and type:

```js
window.history.state;
// Object { goal: "Learn about pushState()" }
```

If you now use the **window.history.back()** function you will not go back to
the previous page, but your URL address will return to the original URL address.
If you use **window.history.forward()** you will move back to our new URL that
ends in **new-state**

We have now successfully implemented a basic version of **Client-Side** routing.

As we start learning about **React Router** we will start implementing
**pushState()** within the context of a **React** app.

### React Router: v5 and v6

React Router released [version 6][] in November of 2021, and the new version has
several changes to the way React Router works internally as well as how you can
use it as a developer. Our curriculum teaches version 5 of React Router, since
it's more widely used by the React community at this time. You can access the
documentation for React Router v5 here:

- [https://v5.reactrouter.com](https://v5.reactrouter.com)

While the React community transitions from using v5 to v6, you'll likely come
across blog posts and other resources that refer to both versions. Make sure to
look for resources that refer to v5 of React Router for consistency with the
syntax we cover in the curriculum. You're welcome to explore React Router v6 if
you're curious! Just be aware of these changes.

## A Word About Accessibility

The web was designed, from its inception, to be a platform for _everyone_,
including those who need help interacting with it through assistive devices.
Those requiring captions, inverted contrast, etc. have all been able to
participate in the web because it was designed with accessibility in mind _from
the beginning_.

Creating accessible sites using SPA-style applications represents an additional
challenge. Many tutorials breeze past this consideration; if you'd like to
explore this topic more deeply, here's a [blog post][bp] on accessibility in
React.

## Resources

- [React Router Tutorial](https://v5.reactrouter.com/web/guides/quick-start)
- [Manipulating Browser History](https://developer.mozilla.org/en-US/docs/Web/API/History_API)

[bp]: https://blog.usejournal.com/getting-started-with-web-accessibility-in-react-9e591fdb0d52
