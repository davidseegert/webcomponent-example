# WebComponents Example
This basic example shows the usage of webcomponents. It uses the webcomponents v1 (object orientated es6) specification. It is working without additional libraries like x-tag or polymer however the webcomponents.js libraries are included for cross-browser support.

## Files

### x-button.html

``` html
<template id="x-button-template">
  <button id="button">Hallo</button>
  <style media="screen">
    /* Adding some color to our custom button to see if css scoping is working*/
    button {
      color:red;
    }
  </style>
</template>


<script>
    // reference to this document; document._currentScript is a fix for older browsers
    var localDoc = (document._currentScript || document.currentScript).ownerDocument;

    // CSS scoping fix for older browsers
    var templateTest = localDoc.querySelector('#x-button-template');
    ShadyCSS.prepareTemplate(templateTest, 'x-button');

    class xButton extends HTMLElement{
      constructor(){
        // call original constructor by calling super()
        super();
        // create shadow dom
        this.shadow = this.attachShadow({mode: 'open'});
        // select template by id
        var template = localDoc.querySelector('#x-button-template');
        // defining self to call "this" from anonymous functions
        var self = this;
        // append template to shadow-dom
        this.shadow.appendChild(template.content.cloneNode(true));

        //register click event
        this.shadow.querySelector('button').onclick = function(){
          alert(self.message);
        }

      }

      // overload of observedAttributes() function
      // defines which attributes will be watched
      static get observedAttributes() {return ['message']; }

      // watch attribute changes
      attributeChangedCallback(attr, oldValue, newValue) {
        if (attr == 'message') {
          this.message = newValue;
          this.shadow.querySelector('#button').innerHTML = 'Say: ' + this.message;
        }
      }

    }


    // register element
    customElements.define('x-button', xButton);
</script>
```

### index.html
``` html
    <!DOCTYPE html>
    <html>
      <head>
        <!-- fallback for older browsers -->
        <script src="js/webcomponents-lite.min.js"></script>
        <script src="js/custom-elements.min.js" charset="utf-8"></script>
        <script src="js/shadydom.min.js" charset="utf-8"></script>
        <script src="js/shadycss.min.js" charset="utf-8"></script>

        <link rel="import" href="x-button.html">
        <meta charset="utf-8">
        <title>Custom Element</title>
      </head>
      <body>
        <x-button id="my-x-button" message="Hello World!"></x-button>
        <!-- Another Button to test CSS scoping. It also changes the message value -->
        <button type="button" name="button" onClick="document.getElementById('my-x-button').setAttribute('message',Math.random())" >Real Button</button>
      </body>
    </html>
```
