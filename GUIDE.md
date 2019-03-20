---
"title": "Onsen UI Guide"
---

## Some subtitle

Here is some text about something

> Here is a warning. We're going to set it as a block quote.

```javascript
const () => console.log('and here is some code');
```


## Installing and Running

- Is the original not OK here?
  - NO! Because it doesn't shill Monaca. And anyway, Monaca debugger is very
    handy here.

- Installation options:
  - Run in playground
    - No setup required
    - Easy to switch between iOS and Android
    - Can't do multiple files
  - Monaca CLI 
    - **RECOMMEND THIS ONE**
      - Will I need to update the templates before I recommend this? If the
        templates change then the tutorial might be in hot water.
    - Can develop locally with Monaca debugger which is very good when it
      doesn't break
    - Need to create a Monaca account
    - You will end up with a Cordova app but if you don't know what that is, you
      can safely ignore it for this tutorial
    - Get node and then:

    ```
    npm install -g monaca
    monaca create tutorial --template onsenui-v2-js-minimum
    ```

  - Regular old CDN
    - Good if you live in 2004; bad if you aren't sold on self-flaggelation
    - Joking aside, this is a fine option, but you'll need to figure out how to
      make it into an app
  - JS Frameworks
    - If you want to use React, Vue or Angular 2+, see the tutorial I haven't
      written yet
    - If you want to use AngularJS, see the tutorial that will never be written

## Explanation of project structure

## Login page

Let's create the first screen. Each screen in Onsen UI is called a "page". The
Onsen UI page component is called `ons-page`. Add the following in the body of
`index.html`:

```html
<body>
  <ons-page></ons-page>
</body>
```

Run the app on your device or browser and you will see screen is a light gray
color. This is the `ons-page` component.

Typically, everything that you want to display in a page should go within
`ons-page` tags. This way Onsen UI will automatically position your elements
correctly.

### Creating a login screen

The first thing you want to show a user when they open your app is probably a
login screen. We can create one very easily using Onsen UI's form components.
These form components work mostly the same way as regular HTML form elements
such as `input`, but the Onsen UI ones will be automatically styled to match the
device your app is running on.

Add this to index.html:

```html
<body>
  <ons-page>
    <div style="text-align: center; margin-top: 50%">
    <p>
      <ons-input id="username" placeholder="Username" modifier="underbar"></ons-input>
    </p>

    <p>
      <ons-input
        id="password"
        placeholder="Password"
        type="password"
        modifier="underbar"
      >
      </ons-input>
    </p>

    <p>
      <ons-button>Sign in</ons-button>
    </p>
    </div>
     
  </ons-page>
</body>
```

Most of this should look familiar to you as regular HTML with some styling, but
notice the two new Onsen UI components: `ons-input` and `ons-button`.

`ons-input` is an input box that can be used the same way as an HTML `input`.
For example, we have set `type` and `placeholder` attributes just like you would
with an HTML `input`. The same goes for `ons-button` which is the Onsen UI
equivalent of `button`.

#### Modifiers

Notice that we have set the `modifier` attribute of the two input boxes.
`modifier` allows you to easily change the appearance of an Onsen UI component.
See the API pages for details of what modifiers are available. Try using them in
this login form to change the appearance of the button and input boxes.

> > - Open up `www/index.html`
> > - In the body, add:
> > 
> > ```html
> > <ons-page>
> >   Hello, world!
> > </ons-page>
> > ```
> > 
> > - Say some things about `ons-page` and how it's HTML just like you know and love
> > - Now stick in the login form and button:
> > 
> > ```html
> > <ons-page>
> >   <div style="text-align: center; margin-top: 50%">
> >   <p>
> >     <ons-input id="username" placeholder="Username" modifier="underbar"></ons-input>
> >   </p>
> > 
> >   <p>
> >     <ons-input
> >       id="password"
> >       placeholder="Password"
> >       type="password"
> >       modifier="underbar"
> >     >
> >     </ons-input>
> >   </p>
> > 
> >   <p>
> >     <ons-button style="">Sign in</ons-button>
> >   </p>
> >   </div>
> > </ons-page>
> > ```
> > 
> > - (Boy, it certainly got ugly quickly.)
> > - Run the app, punch the air


### Adding JavaScript

We have a pretty login form now, but when we click the button, nothing happens.
To that fix let's add a bit of JavaScript. In the `script` tags in the head of
`index.html`, add this:

```html
<script>
  const login = () => {
    const username = document.querySelector('#username');
    const password = document.querySelector('#password');

    if (username === 'USERNAME' && password === 'PASSWORD') {
      ons.notification.alert('Correct!');
    } else {
      ons.notification.alert('Wrong username/password combination');
    }
  }
</script>
```

> This tutorial uses arrow functions insted of the `function` keyword.

Then set the button to run the `login` function when it is clicked:

```html
<ons-button onclick="login()">Sign in</ons-button>
```

Run the app again. Enter a username and password and click 'Submit'. An alert
will pop up telling you whether your login was successful.

In plain JavaScript we would use `alert` to show an alert, but Onsen UI has its
own notifications. There are several different notifications available, but the
one used here is `ons.notification.alert`. See more about the available
notifications in the API pages.

> > - Now make the button work
> >   - Introduce the `script` tag
> >   - Set `onclick` and say it's just exactly the same as a normal button but it
> >     looks better. Make the click show an Onsen alert.

## Navigator and logging in for real(-ish)

All well and good. But we'd like to move to a new page once the user
successfully logs in, so let's create a new page.

### Templates

Add the following to `index.html`, _outside_ the `ons-page` tag:

```html
...
</ons-page>

<template id="home.html">
  <ons-page id="home">
    Hello!
  </ons-page>
</template>
```

This is a template for a new page. Templates aren't loaded automatically when
the app starts and will only be created when the page is shown.

### Navigator

To move between pages, use the `ons-navigator` component. `ons-navigator`
provides methods for pushing or popping pages to and from the **page stack**.
The page stack is a stack of screens that represents the order of pages in the
app. For example, when the first page is shown, that page is the only page in
the page stack. If you navigate to a new page, the new page is pushed on top of
the page stack, leaving two pages total on the stack. If you go back, the second
page is popped from the page stack and the first page is shown again.

`ons-navigator` wraps an initial page. This lets it know which page it should
show at startup. Put the `ons-navigator` around the login page:

```html
<body>
  <ons-navigator id="navigator">
    <ons-page>
      <div style="text-align: center; margin-top: 50%">
      <p>
        <ons-input id="username" placeholder="Username" modifier="underbar"></ons-input>
      </p>

      <p>
        <ons-input
          id="password"
          placeholder="Password"
          type="password"
          modifier="underbar"
        >
        </ons-input>
      </p>

      <p>
        <ons-button>Sign in</ons-button>
      </p>
      </div>
       
    </ons-page>
  </ons-navigator>

  <template id="home.html">
    <ons-page id="home">
      Hello!
    </ons-page>
  </template>
</body>
```

Run the app. It looks the same as before, but now we can get the navigator to
show the second page (the one in the template) once the user has logged in.
Change the definition of the `login` function:

```javascript
const login = () => {
  const username = document.querySelector('#username');
  const password = document.querySelector('#password');

  if (username === 'USERNAME' && password === 'PASSWORD') {
    const navigator = document.querySelector('#navigator');
    navigator.resetToPage('home.html');
  } else {
    ons.notification.alert('Wrong username/password combination');
  }
}
```

Run the app, enter the correct username and password, and tap the login button.
The home page will be shown.

> In real life, don't actually put your username and password in HTML because
> anyone can see what they are by looking at the page source. For the purposes
> of this tutorial though, it's fine.

Let's examine the code we just added. We get a reference to the navigator and
then call its `resetToPage` method with the argument `'home.html'`.
`resetToPage` removes all pages from the page stack and then adds the home page
to the stack. We want to remove the login page from the stack so that the user
can't accidentally go back to the login page when she is already logged in.

The argument `home.html` refers to the page to reset to. The page name is the
`id` of the `template`. By convention, templates that define pages should have
an ID with the suffix `.html`. We'll see why later.

`resetToPage` is just one of the handy methods `ons-navigator` provides. To see
the others, look at the API docs.

### Multiple files

`index.html` is starting to get a little big now and will soon become hard to
maintain. To that end, we can move the home page template to its own file.

Create a new file called `home.html` in the `www` directory, the copy and paste
the __body__ of the home page template from `index.html` (_not including the
`template` tags themselves_).

```html
<ons-page id="home">
  Hello!
</ons-page>
```

Delete the template from `index.html` and the run app. Everything should still
work as before.

This brings up an important point: Onsen UI pages can be defined either as
templates using the `template` element, _or_ in their own file. When we call
`ons-navigator.resetToPage` with `home.html` as the argument, the navigator
looks in the current file for templates with `id="home.html"`, and then looks
for files called `home.html`. This is why nothing broke when we moved the home
page out of a template and into its own file.

> > - Talk about pages
> > - Make a new page
> >   - Recommend now that any children still in the playground should grow up and
> >     get a real man's toolchain
> >   - The new page should go in a new file
> > 
> > - The mighty navigator! (or how to wield pages)
> > - Move the login page to its own file and make the index page just the navigator
> > 
> > - Now to log in:
> >   - Set the click for button to check the username and password and then move
> >     to the new page if all is good
> >   - At this point, warn any morons that you shouldn't store the username and
> >     password in your HTML in real life

## Splitter menu to About page

## Toolbar and back button

## Multiple files

- Actually we might be able to put this earlier

## Tabs

- Add a new tab for the pokemon list page

## Harcoded list

- Do expandable list with Save button here
  - But wait, that's bad UI! (Well, that's a problem for U, not I.)

- Add save functionality now or once we have the grid?

## Grid

## Page data from server

- Add local storage caching as well
- And don't forget the button to clear local storage as an aside

## Card carousel
