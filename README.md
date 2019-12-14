# Critical Render Path


When you open a website, a few things happen in the background, specifically in the Browser. The first step is loading the HTML file from the server and create a DOM (Document Object Model). This Object tells the Browser the content of the website. After this, a CSS file is loaded creating the CSSOM, another object that tells the Browser which style to apply. This phase, the JS file. With the DOM and CSSOM, a Render Tree is created. The layout is shown on the page and finally, the pixels are painted with colours. All these processed happen in few milliseconds, without you even notice, and together form the Critical Render Path.

1. DOM
2. CSSOM
3. Render Tree
4. Layout
5. Paint

## Why is so important?

An understanding of The Critical Render Path helps you to pay more attention to how to create things in your website and where to place it. An example could be when is the best moment to load your script or style.

### SAD PRACTICE: Script in the <head>

Placing the script the `<head>` of an HTML could cause latency and slows the flow of a website. In the example below you can see that the script prevents from loading the CSS file (pending), that should be one of the first things that must be loaded.


![pending_css](/img/a.png)

### AWESOME PRACTICE: Script in the <head>

That's why you always should place the script at last, before the `</body>`. Placing the script at the end allows the website to load the style at first. That why you want to put the style in the `<head>`. And as you can see from the image below the HTML file and the CSS file are loaded and the Object Models created.


![pending_css](/img/b.png)


## Exception

An exception could be the Google Analitycs script that must be placed in the `<head>` in order to track all the user behaviours from the first load of the page to the sad click of the red button.
