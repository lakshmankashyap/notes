# Flexbox

It gives the container the ability to expand and shrink elements to best use all the available space.
It also replaces float layouts and is great when designing 1D layouts.

## Properties

### Container

Main axis : from left to right

Cross axis : from top to bottom

* **flex-direction** : specify in which direction the main axis goes
    * *row*
    * row-reverse
    * column
    * column-reverse
* flex-wrap : defines if items should wrap into a new line if there is not enough space
    * *nowrap*
    * wrap
    * wrap-reverse
* **justify-content** : how items will be aligned along the main axis
    * *flex-start*
    * flex-end
    * center
    * space-between
    * space-around
    * space-evenly
* **align-items** : how items will be aligned along the cross axis
    * *flex-start*
    * flex-end
    * center
    * baseline
* align-content : applies when there is more than one row of flex items
    * *stretch*
    * flex-start
    * flex-end
    * center
    * space-between
    * space-around

### Items

* align-self : similar to align-itmes but for one individual Flex item. For example, if we set align-items to center and we want an element to be at the bottom, we use align-self.
    * *auto* 
    * stretch
    * flex-start
    * flex-end
    * center
    * baseline
* order : defines in which the Flex item should appear inside the container, great to reorder items for smaller screens
    * 0
    * 1
    * ...
* flex-grow : defines how much an item can grow
    * 0
    * 1
    * ...
* flex-shrink : defines how much an item can shrink
    * 0
    * 1
    * ...
* flex-basis : defines the base width of an item
    * auto
    * ...
* flex : shorthand property for the last three properties
    * 0 1 auto
    * ...
    
### Difference between space-between and space-evenly1 - space-evenly :  the items are evenly distributed within the alignment container along the main axis. The spacing between each pair of adjacent items, the main-start edge and the first item, and the main-end edge and the last item, are all exactly the same.

* space-evenly : the items are evenly distributed within the alignment container along the main axis. The spacing between each pair of adjacent items, the main-start edge and the first item, and the main-end edge and the last item, are all exactly the same.

* space-between : very similar to space-evenly  in that the flex-items are evenly distributed within the alignment container along the main axis; however, the first and last flex-items are flushed (pressed without any space) to the main-start edge and main-end edge respectively.
