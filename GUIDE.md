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

Add this to `index.html`:

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

> It's quickly going to get annoying if we have to keep typing in the username
> and password every time we want to run the app. While you're developing, set
> the correct username and password to empty strings to save time.

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

## Toolbar

The home page could use a little more styling. Let's add a toolbar to make it
look better. For this we need the `ons-toolbar` component:

```html
<ons-page id="home">
  <ons-toolbar id="home-toolbar">
    <div class="center">Home</div>
  </ons-toolbar>

  Hello!
</ons-page>
```

Here we have used `ons-toolbar` to define a toolbar. Inside it we have a `div`
with class `center`. `div.center` describes what should go in the middle of the
toolbar. You can also use `div.left` and `div.right` to position elements to the
left and right of the toolbar.

## Splitter menu to About page

Now let's see how to add a collapsable side-menu. We're going to add links to
other pages in the side-menu, so let's create a new page. Create a new file
`about.html` and paste this:

```html
<ons-page id="about">
  <ons-toolbar>
    <div class="center">About</div>
  </ons-toolbar>

  This is the about page.
</ons-about>
```

We'll also need a button to open the side-menu, so let's add one to the left of
the home page toolbar, complete with a function that we'll define in a minute:

```html
<ons-toolbar id="home-toolbar">
  <div class="center">Home</div>

  <div class="left">
    <ons-toolbar-button oncick="openMenu()">
      <ons-icon icon="md-menu"></ons-icon>
    </ons-toolbar-button>
  </div>
</ons-toolbar>
```

Two new components worth mentioning here:

Firstly, `ons-toolbar-button`: This component is basically the same as
`ons-button` except that it is specifically for buttons inside a toolbar. It
adds some extra styling to make the button fit the look of the toolbar.

### Icons

Secondly, `ons-icon`: Whenever you want to display an icon, use this component.
The specific icon is defined in the `icon` attribute. There are Ionicons for
iOS, Material Design icons for Android, and Font Awesome icons. Each type of
icon has its own prefix: `ion-`, `md-`, and `fa-`, respectively. For examples of
`ons-icon` usage, see the API page.

### Splitter

Side-menus are created in Onsen UI by using the `ons-splitter-` components.
There is a parent component `ons-splitter`. It has two children:
`ons-splitter-side` which contains everything that should appear in the
side-menu; and `ons-splitter-content` which defines everthing _outside_ the side
menu. This means that `ons-splitter` effectively wraps the whole app.

We're about to rewrite the body of `index.html` so first let's move the login
page to a new file `login.html`:

```html
<ons-page>
  <script>
    const login = {
      const username = document.querySelector('#username').value;
      const password = document.querySelector('#password').value;

      if (username === '' && password === '') {
        const navigator = document.querySelector('#navigator');

        navigator.resetToPage('home.html', { animation: 'fade' });
      } else {
        ons.notification.alert('bad');
      }
    };
  </script>

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
```

Notice that we also copied the `login` function and put it in a `script` tag.
Strictly this isn't necessary but it's good practice to keep helper functions
with the HTML they are called from.

Now change the body of `index.html` to:

```html
<body>
  <ons-splitter>
    <!-- The side-menu -->
    <ons-splitter-side id="menu" collapse>
    </ons-splitter-side>

    <!-- Everything not in the side-menu -->
    <ons-splitter-content>
      <ons-navigator id="navigator" page="login.html"></ons-navigator>
    </ons-splitter-content>
  </ons-splitter>
</body>
```

If you open the app now, you won't see the side-menu. We need to add a way to
open it. We don't want to be able to open the side-menu from the login page, but
we do want to be able to open it from the home page. Now it's time to define the
`openMenu` function we added to the toolbar button in `home.html`:

```html
<script>
  const openMenu = () => {
    document.querySelector('#menu').open();
  };
</script>
```

Run the app, hit the login button, add the tap the menu icon at the top left of
the home page. The side-menu appears!

But there's nothing in it, so let's remedy that now. We are going to put a list
of links in the side-menu, and for that we need `ons-list` and `ons-list-item`:

```html
<ons-splitter>
  <ons-splitter-side id="menu" collapse>
    <ons-page>
      <ons-list>
        <ons-list-item onclick="loadPage('about.html')">
          About
        </ons-list-item>
      </ons-list>
    </ons-page>
  </ons-splitter-side>

  <ons-splitter-content>
    <ons-navigator id="navigator" page="login.html"></ons-navigator>
  </ons-splitter-content>
</ons-splitter>
```

`ons-list` represents a list - it's the Onsen UI equivalent of `ul`.

`ons-list-item` is a single item in a list. It is only ever used inside an
`ons-list`. It's the Onsen UI version of `li`. Like the toolbar, elements can be
position to the left, right and center of `ons-list-item` with `div.left`,
`div.right` and `div.center`. If you don't define one of these, the list item's
contents are positioned in the center by default.

In the `onclick` attribute of the list item we just defined, we've called a
function `loadPage`. We need to define it:

```javascript
const loadPage = (page) => {
  document.querySelector('#menu').close();
  document.querySelector('#navigator').bringPageTop(page, { animation: 'fade' });
};
```

Now when the side-menu is opened, it will contain a link to the About page. Tap
the link and the About page will be loaded, then the side-menu will close.

> The `loadPage` function uses `ons-navigator.bringPageTop`. This function
> pushes a page to the top of the page stack. If the page was already in the
> page stack, `bringPageTop` takes it from wherever it was and places it at the
> top. Otherwise, it creates a new instance of the page. `ons-navigator` has
> another method, `pushPage`, which creates a new instance of a page regardless
> of whether it already exists in the stack. We recommend using `bringPageTop`
> over `pushPage` to avoid the error of accidentally loading the same page
> twice, which can cause conflicts and unexpected bugs.

## Back button

Let's stop and see how far we've got:

  1. We can log in to the app.
  2. Once successfully logged in, the home page is shown.
  3. From the home page, we can open the side menu and navigate to the About
     page.

Very nice, but once the user is on the About page, there's no way to get back
to the home page. We could add a button and hard-wire it to go to the Home page,
but a better solution is to add a __back button__.

The back button component is `ons-back-button`. When `ons-back-button` is
tapped, it looks for a parent `ons-navigator`. Then it pops the top page off
the navigator's page stack, taking the user back to the previous page.

Let's add it to the About page toolbar:

```html
<ons-page id="about">

  <ons-toolbar>
    <div class="left">
      <ons-back-button></ons-back-button>
    </div>

    <div class="center">About</div>
  </ons-toolbar>

  <p>The mighty About page.</p>

</ons-page>
```

No further work is needed; `ons-back-button` works straight out the box.

## Tabs

Along with `ons-navigator`, Onsen UI provides another component for handling
screen navigation. This is the `ons-tabbar` component. The tabbar keeps track of
one or more tabs. It looks just like the tabbar in a web browser.

Most apps will make use of both `ons-navigator` and `ons-tabbar`. As a rule of
thumb, the most commonly used pages should be tabs, and the less frequently
visited pages (such as the About page) should not be.

We're next going to create a page that displays a list of Pokemon. Create a new
file `pokemon.html` and paste the following:

```html
<ons-page id="pokemon">

  <script>
  </script>

  <ons-list>
    <ons-list-item>bulbasaur</ons-list-item>
    <ons-list-item>charmander</ons-list-item>
    <ons-list-item>squirtle</ons-list-item>
    <ons-list-item>pikachu</ons-list-item>
    <ons-list-item>trubbish</ons-list-item>
  </ons-list>

</ons-page>
```

Later we'll also add functionality to save Pokemon, so create a file
`saved.html` for that now too:

```html
<ons-page id="saved">

  <script>
  </script>

  <p>Saved Pokemon go here.</p>

</ons-page>
```

OK, now back to `home.html` for the tabbar. Remove the `"Hello!"` message and
replace it with `ons-tabbar`. (Don't miss the toolbar title changing from "Home"
to "Pokemon"):

```html
<ons-page id="home">
  <script>
    ...
  </script>

  <ons-toolbar id="home-toolbar">
    <div class="center">Pokemon</div>

    <div class="left">
      <ons-toolbar-button oncick="openMenu()">
        <ons-icon icon="md-menu"></ons-icon>
      </ons-toolbar-button>
    </div>
  </ons-toolbar>

  <ons-tabbar id="tabbar">
    <ons-tab page="pokemon.html" label="Pokemon"></ons-tab>
    <ons-tab page="saved.html" label="Saved"></ons-tab>
  </ons-tabbar>
</ons-page>
```

Run the app. On the home page, there is a tabbar with two tabs labelled
"Pokemon" and "Saved". Tap the tabs to switch the page content (i.e. the middle
of the screen between the toolbar and the tabbar) between the two pages
`pokemon.html` and `saved.html`.

Let's examine the tabbar markup in more detail. We defined an `ons-tabbar`
element with two `ons-tab` child elements. The only children of an `ons-tabbar`
should be `ons-tab` elements.

Then each `ons-tab` has a `page` attribute and a `label` attribute. The `page`
attribute specifies which page should be loaded when the tab is tapped (this is
the same as the `page` attribute of `ons-navigator`). The `label` attribute
specifies what text is on the tab itself. There are more `ons-tab` attributes;
see the docs.

### Structure of the app

We have now built an app with a fairly complicated structure. Time for a quick
review:

```
At the top level, there is `ons-splitter`.
 |
 |-- The first child of `ons-splitter` is `ons-splitter-side`. Everything
 |   defined in here specifies what should be in the side-menu.
 |
 |-- The second child of `ons-splitter` is `ons-splitter-content`. Everything
     apart from the side-menu is defined in here.
      |
      |-- Within `ons-splitter-side` is `ons-navigator`. The navigator is used to
      |   move between any pages that aren't tabs.
      |
      |-- Within `ons-navigator` are multiple `ons-page` components. Each
          defines a page. `home.html` is the most interesting though, because it
          contains a _tabbar_. Each child of the tabbar is a _tab_.
```

One common source of confusion is mixing up pages controlled by `ons-navigator`
and tabs controlled by `ons-tabbar`. Carefully plan the structure of your app so
you are sure what should be a tab and what should be a regular page.

> If you're brave, it is possible to have a navigator and tabbar both handle the
> same page. However, you are likely to be thanked for your efforts by bugs that
> are tricky to fix, such as when you accidentally load a page in the DOM twice,
> giving you two elements with the same ID. In general, there are better ways
> to handle a page that is sometimes a normal page and sometimes a tab.

> > - Add a new tab for the pokemon list page

## Events

You may have noticed something isn't quite right when you go to the "Saved" tab:
The toolbar title still says "Pokemon" when it should say "Saved". To fix this,
we need a bit of JavaScript to dynamically change the toolbar title when the tab
changes.

When the state of an Onsen UI component changes, such as when the tabbar's
focused tab is change, or when the navigator loads a page, an event is fired.
All we need to do to make use of these events is to add an event listener for
the event we're interested in.

In this instance, we want to listen for `ons-tabbar`'s `prechange` event. The
`prechange` event is fired just before the tabbar moves to a tab. We add an
event listener the same way as we would for any other JavaScript event:

```javascript
document.addEventListener('prechange', ({ target, tabItem }) => {
  if (target.matches('#tabbar')) {
    document.querySelector('#home-toolbar .center').innerHTML = tabItem.getAttribute('label');
  }
});
```

Run the app and witness the dynamically-changing toolbar title.

Now to the code:

  1. `document.addEventListener` is called to add a listener.
  2. The first argument states we want to listen for `prechange`.
  3. The callback function receives an event object as an argument. We need the
     `target` and `tabItem` properties only.
  4. We check if the target - the element that fired the event - is the tabbar.
     This is an important step because more than one Onsen UI component can fire
     an event with the same name. You have to make sure you don't accidentally
     handle the wrong component's events.
  5. If it was actually the tabbar that fired `prechange`, we get the label of the
     new tab from `tabItem`'s `label` attribute, and set that as the center text
     of the toolbar.

For more information on Onsen UI events and what you can do with them, check the
API documentation.

## Expandable list items

We breezed over the Pokemon list page before so we could get on to the tabbar,
but let's go back to it now.

So far there's a list of Pokemon, created using `ons-list` and `ons-list-item`.
We would like to be able to select a Pokemon from this list and save it. This is
a job for __expandable list items__.

An expandable list item is one that increases in size when it is tapped,
displaying content that was initially hidden. An `ons-list-item` is turned into
an expandable list item by adding the `expandable` attribute and adding a child
`div.expandable-content` containing the hidden content.

Make each of the existing list items expandable:

```html
<ons-list>
  <ons-list-item expandable>
    bulbasaur
    <div class="expandable-content">
      <ons-button onclick="savePokemon(this)">Save</ons-button>
    </div>
  </ons-list-item>

  <ons-list-item expandable>
    charmander
    <div class="expandable-content">
      <ons-button onclick="savePokemon(this)">Save</ons-button>
    </div>
  </ons-list-item>

  <ons-list-item expandable>
    squirtle
    <div class="expandable-content">
      <ons-button onclick="savePokemon(this)">Save</ons-button>
    </div>
  </ons-list-item>

  <ons-list-item expandable>
    pikachu
    <div class="expandable-content">
      <ons-button onclick="savePokemon(this)">Save</ons-button>
    </div>
  </ons-list-item>

  <ons-list-item expandable>
    trubbish
    <div class="expandable-content">
      <ons-button onclick="savePokemon(this)">Save</ons-button>
    </div>
  </ons-list-item>
</ons-list>
```

> If it's upsetting to see the same markup repeated five times, don't worry
> because later on we'll be generating all of this with JavaScript.

Tap a list item now and you'll see it expand to reveal the Save button. Tap it
again to close.

### Compilation

Take a look at one of the expandable list items in your browser's developer
tools.

> If you're using Monaca CLI, run `monaca preview` to see the app in browser.

Notice the list item contains two children, `div.top` and
`div.expandable-content`. We know about `div.expandable-content` but where did
`div.top` come from? The answer is that when Onsen UI compiled the list item, it
put everything that wasn't inside `div.expandable-content` into `div.top`.
`div.top` represents the part of the list item that is always shown - the _top_
of the list item.

`div.top` itself can have three children: `div.left`, `div.right` and
`div.center`. Like the toolbar, these position elements to the left, right and
center of the top part.

Almost all Onsen UI components have their structured altered in some way during
compilation. This means you can't assume that whatever is in your HTML file is
what will actually be rendered in the app. However, you can "preempt" the
compiler by writing the compiler output in your HTML file to begin with. This is
helpful whenever you want to access things that the compiler does automatically,
such as adding a top part to an expandable list item.

> > What a dreadful explanation ^

> > - Do expandable list with Save button here
> >   - But wait, that's bad UI! (Well, that's a problem for U, not I.)
> >
> > - Add save functionality now or once we have the grid?

## Grid

## Page data from server

- Add local storage caching as well
- And don't forget the button to clear local storage as an aside

## Card carousel
