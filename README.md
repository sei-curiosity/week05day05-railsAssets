[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Rails Assets

Learn how to properly use CSS, JS and Images with our Rails applications.

## Objectives

By the end of this, developers should be able to:

- Leverage the asset pipeline for managing assets
- Include Sass stylesheets with a project
- Include JS code with a project
- Reference images in HTML and CSS using asset pipeline


## Asset Pipeline

The asset pipeline provides a framework to concatenate and minify or compress JavaScript and CSS assets. It also adds the ability to write these assets in other languages and pre-processors such as CoffeeScript, Sass and ERB. It allows assets in your application to be automatically combined with assets from other gems.

The first feature of the pipeline is to concatenate assets, which can reduce the number of requests that a browser makes to render a web page. Web browsers are limited in the number of requests that they can make in parallel, so fewer requests can mean faster loading for your application.

The second feature of the asset pipeline is asset minification or compression. For CSS files, this is done by removing whitespace and comments. For JavaScript, more complex processes can be applied. You can choose from a set of built in options or specify your own.

The third feature of the asset pipeline is it allows coding assets via a higher-level language, with precompilation down to the actual assets. Supported languages include Sass for CSS, CoffeeScript for JavaScript, and ERB for both by default.

## CSS

The manifest file for CSS is the `app/assets/stylesheets/application.css`.  This can be considered the root file for all of your styles.

To include other files, Rails uses the `*=` notation from sprockets.  Generally an application will by default include:
```
 *= require_tree .
 *= require_self
 ```
 
**`*= require_tree .`**

Include all files in the the `app/assets/stylesheets/` folder.
 
**`*= require_self`**

Include any styles written in the `application.css` itself.  Although for the most part, we should not rely on writing styles in the `application.css` directly because they will override all other styles and can be better organized in other files.

**`*= require books`**

`app/assets/stylesheets/books.scss`

If we do not want to use `require_tree .` we can include files individually by using their filename.  This is also useful for when we need to include files from other libararies that are not in our `app/assets/stylesheets/` folder.

**Pro-Tip**

CSS cascading and specificity can be tricky when all of the styelsheets are compiled to one file. 

Add classes to the body of your `application.html.erb` so that you can namespace your styles with classes:
```html 
<body class="<%= controller_name %> <%= action_name %>">
  <%= yield %>
</body>
```

For the PostsController#Index, this would result in: 
```html
<body class="posts index">
```

So you can style specifically for any `posts` controller views with:
```css
.posts {}
```

Or for specifically the `posts#index` view with:
```css
.posts.index {}
```

This tip will also be helpful if we want controller or page specific JS
```js
$('.posts.index button').click()
```

## JS

The manifest file for JS is the `app/assets/javascripts/application.js`.  This can be considered the root file for all of your javascript.

To include other files, Rails uses the `*=` notation from sprockets.  Generally an application will by default include:
```
//= require rails-ujs
//= require activestorage
//= require turbolinks
//= require_tree .
 ```
 
**`//= require rails-ujs`**
**`//= require turbolinks`**

"Unobtrusive Javascript" uses JavaScript to progressively enhance the user experience for capable browsers without negatively impacting clients that do not support or do not enable JavaScript.  These will allow Rails to make AJAX requests to fetch your pages and assets to enhance user experience ans site performance. 

**`//= require activestorage`**

[Active Storage](https://edgeguides.rubyonrails.org/active_storage_overview.html#direct-uploads), with its included JavaScript library, supports uploading directly from the client to the cloud.

**`//= require_tree .`**

Include all files in the the `app/assets/javascripts/` folder.

**`//= require books`**

If we do not want to use `require_tree .` we can include files individually by using their filename.  This is also useful for when we need to include files from other libararies that are not in our `app/assets/javascripts/` folder.

#### jQuery

Include the [jquery-rails gem](https://github.com/rails/jquery-rails) and `bundle install`.
```rb
gem 'jquery-rails'
```

Require jQuery in your `app/assets/javascripts/application.js`
```
//= require jquery
```

**Pro-Tip** 

```js
// jQuery
$(document).on("turbolinks:load", function() {
   console.log("page content has loaded")
   // add your javascript here
}

// vanilla JS
document.addEventListener("turbolinks:load", function() {
   console.log("page content has loaded")
   // add your javascript here
})
```

## Images

Images should be stored in the `app/assets/images/` folder as an example `app/assets/images/cat.png`.

#### In HTML

Use the `image_tag` with a relative path from the `app/assets/images` folder:
```html

<%= image_tag "cat.png" %>
 ```
 
#### In SASS
 
 Use `image-url` with a relative path from the `app/assets/images` folder:
```css
 body {
    background: image-url('cat.jpg');
}
```
 
#### In JS

Change file type from `.js` to `.js.erb`, as an example `app/assets/javascripts/books.js.erb`.

Use the `asset_path` inside of an erb block.

Example as background image:
```js
$('body').css('background-image', "url(<%= asset_path('cat.jpg') %>)");
```

Example as image source:
```js
$("body").append("<img src=" + "<%= asset_path('cat.jpg') %>" + "/>")
```

## Additional Resources

- https://guides.rubyonrails.org/asset_pipeline.html
- https://guides.rubyonrails.org/working_with_javascript_in_rails.html
- https://mattboldt.com/organizing-css-and-sass-rails/ (how to use sass variables with asset pipeline)
