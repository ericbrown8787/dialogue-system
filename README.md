# JSON Dialogue System

## Introduction

This document describes a proposed standard for organizing and parsing dialogue using a custom JSON format.


## Structure

### Dialogue Files and Blocks

A **dialogue file** is a JSON file which contains an array of JSON objects, referred to here as **blocks**. Each block represents a single, self-contained section of conversation, with no branching choices in the middle.
Each conversation with an interactable character in a scene should be contained within a single dialogue file. 

Blocks are structured as follows:

```json
{
    "id":"",
    "dialogue":[],
    "choices":[]
}
```

#### Attributes: 

- `id`:*(string)* (**Required**): An identifier used by the dialogue parsing system to find a specific block of dialogue. 

- `dialogue`:*(array)* (**Required**): An array containing one or more line objects. 

- `choices`:*(string)* (**Required**): An array containing one or more choice objects. 


#### Dialogue File Structure

A dialogue file is a text file. All dialogue files should use the `.json` file extension. Internally, a dialogue file is an array of block objects, separated by commas

```json
[
    {
        "id":"",
        "dialogue":[],
        "choices":[]
    },
    {
        "id":"",
        "dialogue":[],
        "choices":[]
    },
    {
        "id":"",
        "dialogue":[],
        "choices":[]
    },
]
```

---

### Lines

A **line** object represents a single line of dialogue.

```json
{
    "actor":"charcoal",
    "anim":"unhinged",
    "narration":false,
    "internal":false,
    "text": "THE MURRRRRRRRDER!!!!!!",
}
```
#### Attributes:
- `actor`:*(string)* Identifies the actor(character) to be displayed during this line of dialogue. Overrides the previously displayed actor. If a line does not have an actor specified, the previously displayed actor will continue to be displayed.

- `anim`:*(string)* Specifies the animation/sprite to use for the line of dialogue. If a line does not have an animation specified, the current animation will continue if specified, and the default will be used otherwise. 

- `internal`:*(boolean)* If **true**, apply text style for internal monologue

- `narration`:*(boolean)* If **true**, apply text style for narration. 

- `text`:*(string)* (**Required**) The text of the line.

---

### Choices

A **choice** object represents a single dialogue choice. Choices may be structured as followed

```json
{
    "to": "backOff",
    "text": "[Back off] You win this one, bird. For now."
}
OR
{
    "loadScene": "hospital",
    "text": "[Wake up in the hospital]"
}
```

#### Attributes:

- `to` : *(string)* (**Required***) The block to navigate to, if a choice needs to lead to a different block of dialogue. Must equal the value of a valid block ID. 

- `loadScene`: *(string)* (**Required***) Exits the dialogue and loads a different scene. Must equal the identifier for a valid scene.

- `text` : *(string)* (**Required**) The text to display for this choice 

    *A choice object must have **either** a `to` or `loadScene` attribute.

---

## Example

```json
[
    {
        "id": "start",
        "dialogue": [
            {
                "actor": "charcoal",
                "anim": "neutral",
                "text": "Where were you on the night of December 7th?"
            },
            {
                "text": "..."
            },
            {
                "anim": "intense",
                "text": "DUNDUNDUNNNNN..."
            },
            {
                "actor": "bonnie",
                "anim": "animeSweatDrop",
                "internal": true,
                "text": "Here he goes again..."
            },
            {
                "actor": "charcoal",
                "anim": "unhinged",
                "text": "THE MURRRRRRRRDER!!!!!!"
            }
        ],
        "choices": [
            {
                "to": "escalate",
                "text": "[Escalate the situation] JUUUUUSTIIIICE!!!"
            },
            {
                "to": "backOff",
                "text": "[Back off] You win this one, bird. For now."
            }
        ]
    },
    {
        "id": "escalate",
        "dialogue": [
            {
                "narration": true,
                "text": "Detective Cats smashes a nearby bottle of wine on the table, and threatens Birdsby with its jagged edges. Birdsby picks up a barstool. Bonnie draws a pistol. The busboy has a stick of dynamite for some reason. A bar fight ensues."
            }
        ],
        "choices": [
            {
                "loadScene": "hospital",
                "text": "[Wake up in the hospital]"
            }
        ]
    },
    {
        "id": "backOff",
        "dialogue": [
            {
                "actor": "birdsby",
                "anim": "smug",
                "text": "That's what I thought, cat."
            }
        ],
        "choices": [
            {
                "loadScene": "bar",
                "text": "[Leave]"
            }
        ]
    }
]
```

## Resources
[W3 Schools: JSON Tutorial](https://www.w3schools.com/js/js_json_intro.asp)

[Godot Engine Docs: JSON](https://docs.godotengine.org/en/stable/classes/class_json.html)