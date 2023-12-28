# XML Dialogue Markup Spec

## Introduction

This document describes a proposed standard for organizing and parsing dialogue using a custom XML format.

## Example

```xml
<?xml version="1.0" encoding="UTF-8"?>
<block id="start">
    <dialogue>
    <!-- A collection of lines-->
    <!-- The "character" and "anim" attributes are optional, and will change the current character and animation. Each in-game character data object should probably have defaults as fallbacks-->

        <l actor="charcoal" anim="neutral">Where were you on the night of December 7th?</l>

        <l>...</l>

        <l>The night of...</l>

        <l anim="intense">DUNDUNDUNNNNNN...</l>

        <l actor="bonnie" anim="animeSweatDrop" fontStyle="italic">Here he goes again...</l>

        <l actor="charcoal">THE MURRRRRRRRDER!!!!!!</l>

        <l actor="birdsby" anim="armsCrossed">This bird ain't singin' without a lawyer present.</l>

    </dialogue>

    <choices>
        <c to="escalate">
            [Aggressive]JUUUUSTIIIICE!
        </c>
        <c to="backOff">
            [Back Off]You win this one, bird. For now.
        </c>
    </choices>
</block>

<block id="escalate">
    <dialogue>
        <l narration="true">Detective Cats smashes a nearby bottle of wine on the table, and threatens Birdsby with its jagged edges. Birdsby picks up a barstool. Bonnie draws a pistol. The busboy has a stick of dynamite for some reason. A bar fight ensues.</l>
    </dialogue>
    <choices>
        <c loadScene="hospital">[Wake up in the hospital]</c>
    </choices>
</block>

<block id="backOff">
    <dialogue>
        <l actor="birdsby" anim="smug">That's what I thought.</l>
    </dialogue>
    <choices>
        <c fontStyle="italic" exit="true"> [Leave] </c>
    </choices>
</block>
```

## Tags

### Block
```xml
<block> </block>
```

Denotes a block of dialogue. A block can be defined as a single, self-contained unit of dialogue, at the end of which one or more other blocks of dialogue may be reached. 
#### Attributes
- `id` : (**Required**) An identifier used for navigation between blocks via choices. Each block must have a **unique** `id` attribute value. 
---

### Choice
```xml
<c> </c>
```
Denotes a single dialogue choice. Must be contained within a `choices` tag. 
#### Attributes
- `to` : (**Required***) Must equal the value of a valid block ID. 

- `loadScene`: (**Required***) Exits the dialogue and loads a different scene. Must equal the identifier for a valid scene.

- `fontStyle` : Specifies a font style to use. If no font style is specified, the default will be used(Presumably normal weight, non-italic, non-oblique). Possible values include: 
    - bold
    - italic
    - oblique

    \* a `<c>` tag must have **either** a `to` or `loadScene` attribute. 
---

### Choices
```xml
<choices> </choices>
```
Denotes a list of choice(`<c>`) tags. Each `<block>` tag must contain **no more than 1** `<choices>` tag. 

---

### Dialogue
```xml
<dialogue> </dialogue>
```

Denotes a collection of line(`<l>`) tags. Each `<block>` tag must contain **exactly 1** `<dialogue>` tag. Each `<dialogue>` tag must contain **at least 1** `<l>` tag.  

---

### Line
```xml
<l> </l>
```
Denotes a single line of dialogue. Must be contained within a `<dialogue>` tag. 
#### Attributes:

- `actor` : Identifies the actor(character) to be displayed during this line of dialogue. Overrides the previously displayed actor. If a line does not have an actor specified, the previously displayed actor will continue to be displayed.

- `anim` : Specifies the animation/sprite to use for the line of dialogue. If a line does not have an animation specified, the current animation will continue. 

- `fontStyle` : Specifies a font style to use. If no font style is specified, the default will be used(Presumably normal weight, non-italic, non-oblique). Possible values include: 
    - bold
    - italic
    - oblique

- `narration` : If **true**, apply text style for narration. 

## Resources
[W3 Schools: XML Tutorial](https://www.w3schools.com/xml/default.asp)

[Godot Engine Docs: XML Parser](https://docs.godotengine.org/en/stable/classes/class_xmlparser.html)