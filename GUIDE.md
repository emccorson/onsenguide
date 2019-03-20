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

- Open up `www/index.html`
- In the body, add:

```html
<ons-page>
  Hello, world!
</ons-page>
```

- Say some things about `ons-page` and how it's HTML just like you know and love
- Now stick in the login form and button:

```html
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
    <ons-button style="">Sign in</ons-button>
  </p>
  </div>
</ons-page>
```

- (Boy, it certainly got ugly quickly.)
- Run the app, punch the air

- Now make the button work
  - Introduce the `script` tag
  - Set `onclick` and say it's just exactly the same as a normal button but it
    looks better. Make the click show an Onsen alert.

## Navigator and logging in for real(-ish)

- Talk about pages
- Make a new page
  - Recommend now that any children still in the playground should grow up and
    get a real man's toolchain
  - The new page should go in a new file

- The mighty navigator! (or how to wield pages)
- Move the login page to its own file and make the index page just the navigator

- Now to log in:
  - Set the click for button to check the username and password and then move
    to the new page if all is good
  - At this point, warn any morons that you shouldn't store the username and
    password in your HTML in real life

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
