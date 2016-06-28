# < l2t-paper-slider >

Polymer element for displaying slides in a carousel. 
Have a quick look at the [Component page](http://link2twenty.github.io/l2t-paper-slider) 

![Screenshot](https://media.giphy.com/media/3o72Fh8f8VwsTrvSGA/giphy.gif)

## Install with bower

First you need bower, [see their site](http://bower.io/) for details 

```
bower install --save l2t-paper-slider
```

## Examples

Each slide must be within a paper-slide tag, but other than that you have complete control.

```html
  <l2t-paper-slider total-slides="4">
    <paper-slide>#1</paper-slide>
    <paper-slide>#2</paper-slide>
    <paper-slide>#3</paper-slide>
    <paper-slide>#4</paper-slide>
  </l2t-paper-slider>
```
  
You can set a different default start position, the first start postion is 0 (as opposed to 1)

```html
  <l2t-paper-slider total-slides="2" position="1">
    <paper-slide>#1</paper-slide>
    <paper-slide>#2</paper-slide>
  </l2t-paper-slider>
```
  
## Styling

The following custom properties are available for styling:

| Custom property | Description | Default |
|----------------|-------------|-------------|
| --paper-slide-dot | Color of unselected Nav Dot | rgba(255, 255, 255, .5) |
| --paper-slide-dot-selected | Color of selected Nav Dot | #FFF |
| --paper-slide-width | Width of slide container | 100% |
| --paper-slide-height | Height of slide container | 600px |

## Attributes

### Public

| Attribute Name | Functionality | Type | Default |
|----------------|-------------|-------------|-------------|
| totalSlides* | Number for storing total number of slides | Number | 1 |
| autoProgress | Boolean value to state if slides should auto proceed | Boolean | false |
| slideDuration | Number of seconds each slide should remain for | Number | 5 |
| position | Number for storing start position of slides | Number | 0 |
required*

### Private

| Attribute Name | Functionality | Type | Default |
|----------------|-------------|-------------|-------------|
| _totalDots | Array for storing number leading up to totalSlides | Array | [] |

## Methods

### Public

| Method Name | Action |
|----------------|-------------|
| moveNext() | Method for moving to the next slide or back to the first slide |
| movePrev() | Method for moving to the previous slide or to the last slide |

### Private

| Method Name | Action |
|----------------|-------------|
| _autoProceed() | Method for moving automatically ever slideDuration seconds |
| _createDots(t) | Create the nav dots 1 for each slide | alert("You need to set the action attribute") |
| _insRules() | Styles the dots controls animation |
| _listenerInit() | Adds onclick listener To update the position |
| _setStyles() | Styles the slider does slide width and container width |

## Technical Notes

This is just a place for me to note down something I learnt about styling polymer elements with javascript.

In the code below you will see I have inserted style tags into `this.$.stylecontainer` I initially thought this would be sufficient for keeping the style locked to one scope, but it leaked. This meant when more than one version of the element was loaded the last style generated trumped and ruined all the former elements.

There was a solution, though, when an element is generated it is given a class by polymer, `.element-name-number` the number starts at 0 and 1 is added for each repetition of the element. I had to somehow find where this was stored.

There were no doubt easier ways, but I ended up looking through the polymer source code until I came across `_scopeSelector`. This meant I could preface my style rule with `insertRule("."+this._scopeSelector+"` locking it to the scope in which it was generated.

```js
insRules: function() {
  var i;
  var styleEl = document.createElement('style'), styleSheet;
  this.$.stylecontainer.appendChild(styleEl);
  styleSheet = styleEl.sheet;
  for (i = 0; i < this.totalSlides; ++i) {
    styleSheet.insertRule("."+this._scopeSelector+" .slider[data-pos='"+i+"'] .slider__indicator {left: "+(2*i)+"em;right: "+((2*(this.totalSlides-1))-(2*i))+"em;}", 0);
    styleSheet.insertRule("."+this._scopeSelector+" .slider[data-pos='"+i+"'] .slider__slides {-webkit-transform: translateX(-"+(100/this.totalSlides)*i+"%);transform: translateX(-"+(100/this.totalSlides)*i+"%);}", 1);
  };
},
```

I couldn't find anything telling me how to do this online so I hope you find it helpful.

## Credit

I adapted this project from [Tobias Reich](https://github.com/electerious)'s project [Material Slider Dots](http://codepen.io/electerious/pen/JXNEPr/). The code is mostly his, I just made it more dynamic for a polymer enviroment.
