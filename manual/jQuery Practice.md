# jQuery Practice

http://jquery.com/

## [API Documentation](http://api.jquery.com/)

## [W3Schools](https://www.w3schools.com/jquery/)

[jQuery References](https://www.w3schools.com/jquery/jquery_ref_selectors.asp)

[jQuery Selector Tester](https://www.w3schools.com/jquery/trysel.asp)

[jQuery Tutorial](https://www.w3schools.com/jquery/jquery_intro.asp)

### jQuery Tutorial

[Downloading jQuery](http://jquery.com/download/)
```html
<head>
<script src="jquery-3.2.1.min.js"></script>
</head>
```

jQuery CDN
```html
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
</head>
```

```html
<head>
<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js"></script>
</head>
```

The jQuery syntax is tailor-made for **selecting** HTML elements and performing some **action** on the element(s).

Basic syntax is: **$(selector).action()**
- A $ sign to define/access jQuery
- A (selector) to "query (or find)" HTML elements
- A jQuery action() to be performed on the element(s)

You might have noticed that all jQuery methods in our examples, are inside a document ready event:
```javascript
$(document).ready(function(){

   // jQuery methods go here...

});
```

If your website contains a lot of pages, and you want your jQuery functions to be easy to maintain, you can put your jQuery functions in a separate .js file.
```html
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js">
</script>
<script src="my_jquery_functions.js"></script>
</head>
```

# 学习到这！

https://www.w3schools.com/jquery/jquery_traversing_filtering.asp
