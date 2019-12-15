# Critical Render Path


When you open a website, a few things happen in the background, specifically in the Browser. The first step is loading the HTML file from the server and create a DOM (Document Object Model). This Object tells the Browser the content of the website. Then a CSS file is loaded creating the CSSOM, another object that tells the Browser which style to apply. After that phase, it's time for the JS file. With the DOM and CSSOM, a Render Tree is created. The layout is shown on the page and finally, the pixels are painted with colours. All these processes happen in few milliseconds, without you even notice, and together form the Critical Render Path:

![[critical_render_path](/img/d.png)

1. DOM
2. CSSOM
3. Render Tree
4. Layout
5. Paint

## Why is so important?

An understanding of The Critical Render Path helps you to pay more attention to how to create things in your website and where to place it. An example could be when is the best moment to load your script or style.

## Optimize the DOM phase (A)

1. Load the style in the `</head>`
2. Load the script before the `</body>`

### SAD PRACTICE: Script in the head

Placing the script the `<head>` of an HTML could cause latency and slows the flow of a website. In the example below, you can see that the script prevents from loading the CSS file (pending), that should be one of the first things that must be loaded.


![pending_css](/img/a.png)

### AWESOME PRACTICE: Script at last (A1-2)

That's why you always should place the script at last, before the `</body>`. Placing the script at the end allows the website to load the style at first. That why you want to put the style in the `<head>`. And as you can see from the image below the HTML file and the CSS file are loaded and the Object Models created.


![pending_css](/img/b.png)


## Exception

An exception could be the Google Analitycs script that must be placed in the `<head>` to track all the user behaviours from the first load of the page to the sad click of the red button.

## Optimize the CSSOM phase (B)

1. Load only what you need (silly but important!)
2. Above the fold loading (see the example below)
3. Media Attributes
4. Less specificity

### Load only what you need (B1)

You could avoid loading a `CSS` file using Internal CSS or Inline CSS. Those methods are the best option, only when you don't have multiple `HTML` files. It this case you won't load any style file. 

### Above the fold loading  (B2)

The best thing to do when you have multiple `CSS` files is to load them when you need/use it. It's possible to accomplish this task using JS:

```
const loadStyleSheet = src => {

        // Some browser have this functionality 

        if(document.createStylesheet){
            document.createStylesheet(src)
        } else {

            // Others don't so you need to create the tag by yourself
            // < [link] [rel] [type] [href] >

            const stylesheet = document.createElement('link');
            stylesheet.rel = 'stylesheet';
            stylesheet.type = 'text/css';
            stylesheet.href = src;
            document.getElementsByTagName('head')[0].appendChild(stylesheet);
        }
    }
    
// after, you need to load everything in the browser
window.onload = () => {

    console.log('Style loaded!');
    loadStyleSheet('./style3.css');
}

```

First the `HTML` will look like this: 

```
<head>
    <title>Crtical Render Path</title>
</head>

```

After:

```
<head>
    <title>Crtical Render Path</title>
    <link rel="stylesheet" type="text/css" href="style3.css" />
</head>

```

In the image below you can see that the Browser is loading the `./style3.css` only after the element that uses it, is shown. The red line indicates where the loading stops. As you can see it stops right before the `./style3.css`.

![loading](/img/c.png)


### Media Attributes (B3)

You can also use the `media` query inside the `link` tag. Doing so not only will prevent loading a `CSS` file but will also give you the option to tell the Browser when to do it. In the example below, the criteria to match is: `max-width: 800px`. 

```
<!-- B3. Download style, only when the screen is more than 800px -->
<link rel="stylesheet" type="text/css" media="only screen and (min-width:800px)" href="./style2.css">
```

### Less specificity (B4)

Avoid useless nested element, it will end giving less work to do to the Browser:

```
/* SAD Practice */

.header .nav .item div.test {
    color: black;
}

/* AWESOME Practice */

div.test {
    color: black;
}

```