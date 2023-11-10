# Creating an _aura_ following the cursor

As the cursor moves make a semi-transparent aura (glow) around the pointer of the cursor.

## Approach
 - Add an event handler to be triggered when the mouse moves.
 - Use the mouse move event to know the current position of the pointer on the page.
 - Create a `div` `HTMLElement` and position it to the current position of the pointer. Update the position of this div as the cursor moves.
 - Style the div using `radial-gradient` to create the _aura_ effect.
 - Set the `border-radius` on the div to `50%`, effectively making the `div` a circle.

## Code

### HTML
```html
<html>
  <body>
    <div class="aura-container">
        <!-- Will bind the mousemove event listener to this div -->
    </div>
  </body>
</html>
```

### CSS 
```css
.aura-container {
    height: 500px;  /*Giving some height to the container div*/
    border: thin blue dashed; /*A visible border to the div*/
}

/*Will add this class to the dynamically created div
The radial-gradient will create the aura effect we want*/
.aura {
    background: radial-gradient(red, white);
    opacity: 30%; /*Giving some transparency to enhance the aura effect and make the background visible*/
}
```

### JS
```javascript
const onDOMReady = (event) => {
    // First create the div 
    const auraDiv = document.createElement("div");
    
    // Style the div either by setting values on HTMLElement.style property
    // or setting a class attribute with all the styles.
    // Using both methods here.
    auraDiv.style.height = "100px";
    auraDiv.style.width = "100px";
    auraDiv.style.borderRadius = "50%";
    auraDiv.style.position = "absolute";
    auraDiv.setAttribute("class", "aura")

    // Add our aura div to the container
    document.querySelector(".aura-container").append(auraDiv)
    
    // Listen for mousemove event on the container
    document.querySelector(".aura-container").addEventListener("mousemove", mouseMoved);

    function mouseMoved(event){
        //Set the center of the div to the mouse pointer position.
        // pageX/Y on MouseEvent gives the position of the mouse WRT the full webpage.
        auraDiv.style.left = `${event.pageX - 50}px`;
        auraDiv.style.top = `${event.pageY - 50}px`;
    }
};

// Here running the code when the DOM is ready, can be hooked in other ways as required
document.addEventListener("DOMContentLoaded", onDOMReady);
```

## Demo

https://codepen.io/pen?template=YzBVdaX

<p class="codepen" data-height="300" data-default-tab="js,result" data-slug-hash="YzBVdaX" data-user="adityaprasoon" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/adityaprasoon/pen/YzBVdaX">
  Untitled</a> by Aditya Prasoon (<a href="https://codepen.io/adityaprasoon">@adityaprasoon</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>