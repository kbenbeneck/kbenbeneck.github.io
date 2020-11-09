---
layout: post
title:      "JS SPA Round Two"
date:       2020-11-09 16:09:33 +0000
permalink:  js_spa_round_two
---


This Palettes Project is a Single Page Application(SPA)
This SPA integrates HTML, CSS, and JavaScript with a Rails API backend. 
 
Upon sending the http get request for index.html from inside of my project’s frontend directory, the event of loading and parsing the document, calls a function which uses addition assignment to populate the the main element on the doc with some elements 
This html includes my “why”, some detailed guidance (click new) and a gif that I made with Procreate on my iPad. 


To preserve the single page nature of the app, clicking “New” calls a function that clears the main element by assigning the innerhtml to an empty string.

function clearMain() {
    main.innerHTML = ""
}

The main is then populated with form elements including:
 -color and text type inputs 
 -save and reset buttons, a blank div with a class “square" assignment and an empty list(ul) with a “tonesList” id. 

With CSS, elements with the square class (#square) are styled to have equal width and height, be centered on the page, and rotate 45deg. Box shadows are added to create an illusion of depth.

Step One:

Clicking the background color input opens a color picker where the user may choose a color value. The event of having a color value calls an arrow function expression that dynamically changes the background style of the blank, square palette. Having set the text input value equal to the color input value, the text field is populated with the color’s hex value as the input value changes 

Step Two:

Having an input value on the tones color selector, calls a function that creates an li element with class attributes, appends it to the tones list(ul), and styles the background color to =  tone input.value. 
The tone input function is only called when the tones list(ul) has less than 9 children and the last li added does not have the same value.

The reset button simply sets the tones list ul to an empty string and resets the counter that limits 9 children. 

Once save is clicked, the background color value, and all tone li values are noted as key: value pairs using object literal notation. 

    let palette = {
        background: document.getElementById('background').value,
        tones_attributes: []
    }
    let fTones = document.getElementsByTagName('li')
    for (let li of fTones){
        let tone = {
            hex: li.innerText,    
        }
        palette.tones_attributes.push(tone)
    } 

Before requests are sent, we have a palette object 
{background: “#693215”, tones_attributes: Array(9) [{…}{…}{…},…

Having set up active record associations in the rails backend:
A Tone belongs to a Palette
A palette has many tones AND accepts nested attributes for tones
Furthermore, the strong params within the palettes controllers are expecting nested tones.
Nesting the tones as objects(with hex properties) within the palette as tones_attributes, enables the palette to be posted with nested json.


   fetch(PALETTES_URL, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json', 
            'Accept': 'application/json'
        },
        body: JSON.stringify(palette)
    })
    .then(res => res.json())
    .then(palette => {
        clearMain()
        let p = new Pal(palette)
        main.innerHTML += p.renderPalette()
        let pdiv = document.querySelector('div.square')
        pdiv.style.background = pdiv.innerText
        p.renderTones() 
        setTimeout(function() { getPalettes(); }, 3000);
    })
    

When sent as a post request, with the config object outlining the method, type, and stringified data, the palettes controller will create a new palette, assigning it an id. For each tone in the array, new tones are created with that id being assigned to the tone property, palette_id.

I should note that the post request, or any other cross server requests are only possible because of the rack Cors middleware. 
This allows the script to explicitly access the json being rendered from the rails server.
Additionally, the json being rendered passed through a serializer service class that cleans up the json.

Simply put, to place some colors on the Dom, I do not need to know what time it was updated or created at. We don’t flood the response with these values. 

class PaletteSerializer
    
    def initialize(palette_object)
        @palette = palette_object
    end

    def to_serialized_json
        options = {
            include: {tones: {except:[:created_at, :updated_at]}},
            except: [:created_at, :updated_at]
        }
        @palette.to_json(options)
    end
end




After sending the post request, json() is called on the the http response object, making the data consumable json.

Passing the now consumable json as the argument for the class constructor method, initializes a new palette object that has access to the class methods: renderPalette() and renderTones().

These class methods display the data very similar to the form arrangements and styles; however, the box shadows on each tone li element are oriented in a way to give a slight wide angle perspective. I wanted the tones to have a settled or locked in feel. Think - unlocking your phone icons and locking them, minus the animation  

After 3 seconds, the getPalettes() function is called, which clears the main element and fetches the palettes index. 

fetch(PALETTES_URL)
    .then(resp => resp.json())
    .then(palettes => {
        palettes.slice().reverse().forEach(palette =>{
            let pal = new Pal(palette)
            indexDiv.innerHTML += pal.renderIndex()
            
            pal.renderIndexTones()
        })
        attachClickToLinks()
    })

A config object is not required as fetch sends a get request by default. 

Slicing and reversing the palettes response allows them to be iterated from back to front, rendering the last creation first.

Originally, I wanted to arrange the palettes index by simply repeating the post creation view, but I couldn’t help but try to implement some elements of isometric op-art design. 

Each palette tone li is skewed 30 degrees, The illusion is created when three squares turned rhombi join, converging their 3 axes in the same position. This creates a perspective for which the left, right, and top(or bottom depending on your fixation) are all visible. 

Each palette includes a link to each palette’s show page. Having attached event listeners, clicking the palette link, calls a function to fetch that palette by it’s data set id. 

Similar to the creating process, an es6 class object is initialized after formatting the server response. With access to class methods, the Dom is manipulated to display the palette and tones just as they were post form. 

















