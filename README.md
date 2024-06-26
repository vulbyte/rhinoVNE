# rhino-html_visual_novel_engine (rhinoNVE)

---

## description

a simple engine to take basic inputs, load scenes, then call certain things like menus using intuitive functions

---

## goals of the project

- ACKSHUALLY make development of these faster;
- keep it simple stupid;
- keep everything using plain js and nothing fancy. no DSL (domain specific language);
- mobile and desktop support for anything that supports html5 && css3

---

## Startup

### 1. server for testing:

to run local http server for testing:

1. make sure python3 is isntalled
2. run:

```bash
python3 custom_server.py ./index.html
```

### 2. pre place or insert automatically

choose either:

A:
the recommended way to insert rhinoVNE into a scene is pre-place a canvas element with the id "rhino VNE" like:

```html
<canvas id="rhinoVNE"></canvas>
```

or B:

```bash
config.json > "rhinoVNE" > "isPreplaced" = to true
```

NOTE!:

> if preplaced is true, it will assume the container above has all the styling properties you desire. if automatic it'll center the canvas within the div

rhino will then check:

```json
{
    ...
    "display": {
        "resolution": {
            "type": x
        }
    }
    ...
}
```

the 2 options for this setting are:
`auto` and `manual`, if neither are present then the default is "auto".

auto by default will take the window.width and window.height and make the canvas 90%, unless the settings in:

```json
{
    ...
    "display": {
        "auto_defaults:" {
            "x": "90",
            "y": "90"
        }
    }
    ...
}
```

are changed.

rhinoVne expects at least a body element to be present,
if not it will create one (body element) then apply it to the first div within said canvas to it

and f!@#ing wing it

## game starting

after the above the game will launch into the menu, which will look into:

```js
config.json > project_info > title;
```

and display the title, along with the basic menu info

---

## example:

the basic scene looks like this:

```js

    scene-id: {
        scene_info: {
            tod: 1800, //time of day
            prev_scene: "scene-id(-1)",
            next_scene: "scene-id(+1)"
        },
        character1: { //the name can be changes if desired
            name: "vulbyte",
            sprites: "../assets/sprites/vulbyte",
            text_color: "yellow",
        },
        character2: {
            name: "not_vulbyte",
            sprites: "../assets/sprites/not_vulbyte",
            text_color: "red",
        },
        dialog_info: {
            id: "0001-0001", //first number is the scene, 2nd is the dialog number of said scene
            dialog: [
                //this is an array as it will read as from 0, to 1, to 2, and so on.
                //if you want a branch it should be a new scene
                {
                    chars: { //these are the ones that will be visual during the current text box, if they're not listed they will be ignored
                        character1: {
                            sprite: 'tired' //this will add to the end of:
                            // `sprites: "../assets/sprites/vulbyte"`, changing the value to "../assets/sprites/vulbyte/tired", and will attempt to load a file of: gif, svg, png, jpeg, then webp in that order.
                            // if not match is found will reference "GLOBAL_SETTINGS.display_fallback" to determine which of the 3 error types to do:
                            // display the image not found to make error obvious (default),
                            // display nothing,
                            // hard crash
                        }
                    },
                    text: {
                        speaker: "",
                        //if unspecified or no match will be assumed to be a narrator/internal thought.
                        // this will also be the character displayed prominently, if not the "inactive effects" will be applied to all
                        type: "text", // the type specifies what "thing" will be loaded. in this example it's text, which can be used for a range of things such as:
                        /*
                            narration,
                            discriptions,
                            dialog,
                            and anythign else you could use text for
                        */
                        dialog: "it was a nice day outside, vulbyte was getting groceries"
                    }
                },
                {
                    chars: {
                        character1: {
                            sprite: 'surprised'
                        },
                        character2: {
                            sprite: 'siloette'
                        }
                    },
                    text: {
                        speaker: character2, //this will load the color of their text, but not the sprite
                        type: "dialog",
                        dialog: "Hey! long time no see!"
                    }
                },
                //imagine the rest of a conversation here
                {
                    chars: {
                        character1: {
                            sprite: "waving_eyes_closed"
                        },
                        character2: {
                            sprite: "smiling"
                        }
                    },
                    text: {
                        speaker: character2,
                        type: "dialog",
                        text: "it was great meeting you! i'll talk to you later!"
                    }
                },
                {
                    chars: {
                        character1: {
                            sprite: "thinking"
                        },
                        character2: {
                            sprite: "smiling"
                        }
                    },
                    text: {
                        speaker: character1,
                        type: "option", //this is a type to trigger an option, you can use this for dialog, options, choices, etcetc
                        options: [ //this can take as many inputs as you like and will generatively fill these in, tho the ui is only designed to ever support upto 4 options, so any more will create a scrolling option box which is NOT recommended
                            option1: {
                                prompt: "same! look forward to seeing you sometime soon!",
                                next_scene: "../scenese/0002-a.json",
                            },
                            option2: {
                                prompt: "it was okay, i guess i'll seeya around",
                                next_scene: "../scenese/0002-b.json",
                            },
                            option3: {
                                prompt: "i hope to never see you again",
                                next_scene: "../scenese/0002-c.json",
                            },
                            option4: {
                                prompt: "i would rather die then ever be in the same room as you again",
                                next_scene: "../scenese/0002-d.json",
                            },
                        ]
                    }
                },
            ],
        }
    }
```

---

## FAQ:

- why not use typescript?
  using a weakly typed language, your inputs should always be checked and sanitized, never expect your inputs to be complaint.

---

## IMPORTANT!

- due to this beign pre-1.0, there are many features that are inconsistent/missing. such as: testing currently only supports global testing, and not partial testing.
