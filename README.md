# Really Simple Svelte Routing

Routing is a key feature on any web page, routing is used to display content to the user based on selections the user makes, for example when selecting a menu option. There are many routing components available but sometimes a very simple routing option is needed and the routing components with all their features may be overkill.
This tutorial will show you a very simple way to add routing into a Svelte single page app. The functionality for the routing is all in one file. In this tutorial we will place it in the main page, but it could easily be extracted out of the main page into it’s own component.

## Create Project

To start with lets create a simple Svelte project using one of the base templates. We will be changing all the content on the page but this is the easiest way to get a Svelte project started and running. 
Create Svelte project from a simple Svelte template
```
npx degit sveltejs/template svelte-spa-router   
```

Install dependencies
```
npm i
```

Start the application
```
npm run dev
```

## Page Layout
Our page layout is going to be a simple two column layout with the menu in the left hand column and the right hand column will be used to display the content for the menu option chosen. A CSS framework could be used for the columns, but for this tutorial we will stay with custom CSS classes instead of creating a dependency on a third part library.
In the App.svelte page add the following styles. Svelte allows styles to be applied per component.

```css
<style>
.row {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  width: 100%;
}

.column {
  display: flex;
  flex-direction: column;
  flex-basis: 100%;
  flex: 1;
}
</style>
```

Now that we have the styles for a 2 column layout lets create the html for the page. Initially the 2 columns will just display simple headers, but we will replace these later as we build our routes.

Replace all the HTML 
```html
<main>
    <div class="row">
        <div class="column">
            <h1>Menu</h1>
        </div>
        <div class="column">
            <h1>Content</h1>
        </div>
    </div>
</main>
```

If you are running the development server you should now see the two column display with the headers.

## Create a Menu
Let us add a menu in the left hand column using anchor links, replace the text in the first column with our menu
```html
            <h1>Menu</h1>
            <a href="#home">Home</a>
            <a href="#red">Red Page</a>
            <a href="#green">Green Page</a>
```

This menu uses Location hashes to define the page to be display. When the user click s a menu option the page URL will update to include the hash value. We will then get the hash value from the URL, and based on the selected menu option we will display the relevant page
Get the menu selection
To get the menu selection we need to get the page from the current page.
```javascript
<script>
let page = document.location.hash;
</script>
```

This will extract the page hash from the URL, but we also need to get the page whenever it changes such as when the user selects a menu option
```javascript
window.onpopstate = function(event) {
        page = document.location.hash;
    };
```

Now the current location hash is in our page variable both when the user accesses our page with an existing hash value (such as from a bookmark) and when the user clicks one of the menu options.

##  Add the Routing
Based on the page variable we now want to change the content in the right hand pane based on the page that was selected. Replace the Content column contents with this script

```html
            {#if page==="#home"}
              Home Page
            {:else if page === "#red"}
              Red page
            {:else if page === "#green"}
              Green page
            {:else}
              404: Page not Found
            {/if}
```            

In the else section we can choose to show whatever page we want to show. In the example above we show an error page, we could have easily shown the home page, maybe even passing a property through to indicate to the user that the request ed page does not exist
Now when you click on the menu options the content on the right will change to display the selected content. At the moment the content is just a simple text string but could be replaced by another component.
Creating the Red Page
To show that routing can be triggered from anywhere we can add a hash link on any page and the routing will still pick it up.
Create a new component called red.svelte. Add the following to the component

```html
<div>
    <h1>This is the Red page</h1>
    <a href=”#green”>Change to Green Page</a>
</div>

<style>
div {
    background: red;
    color: white;
}
</style>
```

Now replace the “Red page” text in the main page with <Red /> (remember to import the red page into the file).
Now choosing red from the menu will diaply the red page content, and from the red page selecting the option to change t the green page will display the green page, as though the green menu option was selected.
