Language: English | [简体中文](README-ZH.md)

This article introduces how to create a resource pack for MVZ2.

You can download this repository, where some folders provide some necessary resources:

- `examples`: Resource pack examples. You can follow these examples to develop your resource pack and study their file structures.
- `templates`: Necessary texture templates, translation text templates, and English translation texts used as a reference.
- `resources`: All texture and audio resources of the original game.

# Information File
The information file is located in the root directory of the resource pack, and the filename is `pack.json`.

Its content format is generally as follows:

    {
        "name": "Resource Pack Name",
        "author": "Author Name",
        "description": "Resource Pack Description",
        "dataVersion": 0,
        "version": {
            "major": 0,
            "minor": 0
        }
    }

Where:

| Property | Type | Description |
| --- | --- | --- |
| `name` | String | The name of the resource pack, used to display on the resource pack interface. |
| `author` | String | The author name of the resource pack, currently has no effect. |
| `description` | String | A brief description of the resource pack, used to display on the resource pack interface. |
| `dataVersion` | Integer | The data version of the resource pack, used for format checking between different versions of the game.<ul><li>Currently, the data version of the original game is 1.</li><li>If `dataVersion` is lower than this value, it will prompt that the resource pack is outdated, but it will still attempt to load.</li><li>If `dataVersion` is higher than this value, it will prompt that the resource pack is incompatible, and it will not attempt to load.</li></ul> |
| `version` | Object | The version number of the resource pack, currently has no effect. |

**A resource pack must have an information file to be recognized by the game. A resource pack without an information file is invalid.**

# Icon
The icon file is located in the root directory of the resource pack, and the filename is `pack.png`.

It is recommended to use a square texture as the icon for the resource pack.

# File Structure
In addition to `pack.json` as the information file and `pack.png` as the icon, the content manifests of the resource pack are placed in the root directory, while the specific contents are placed in the `assets` folder.

## Content Manifests

A resource pack can contain various content manifests, all of which need to be placed in the root directory, including:
- `textures.json`: Used to manage textures.
- `localization.json`: Used to manage localization text files.
- `audios.json`: Used to manage sound effects and music.
- `fragments.json`: Used to manage the particle colors dropped when machines and some other entities are damaged.
- `characters.json`: Used to manage characters in dialogues.

### textures.json

An example of the content structure of `textures.json` is as follows:

    {
        "textures": [
            {
                "texture": "card_mobile.png",
                "key": "mvz2:ui/level/card_mobile",
                "slices": [
                    {
                        "width": 120,
                        "height": 55
                    },
                    {
                        "x": 121,
                        "width": 120,
                        "height": 55
                    },
                    {
                        "x": 242,
                        "width": 120,
                        "height": 55
                    },
                    {
                        "x": 363,
                        "width": 120,
                        "height": 55
                    }
                ]
            },
            {
                "texture": "card.png",
                "key": "mvz2:ui/level/card",
                "slices": [
                    {
                        "single": true
                    }
                ]
            }
        ]
    }

Properties include:
| Property | Type | Description |
| --- | --- | --- |
| `textures` | List | A list of textures. |

Under `textures` is a list where each element represents an individual texture file. Properties include:
| Property | Type | Description |
| --- | --- | --- |
| `texture` | String | The file path of the texture relative to the `assets` folder. For example, if it is `assets/card_mobile.png`, it should be written as `card_mobile.png` here. |
| `key` | String | The key of the in-game texture to be replaced. Specific keys of textures can be checked in the `resources/textures.json` file of this project.<br>This property will be used for all `slices` in the texture and will automatically assign keys to them, unless the slice itself implements the `key` property.<br>You can use `@` seperator to add tags for this key, such as `mvz2:ui/level/card_mobile@en-US`. Valid tags are as follows:<ul><li>Language Code: Such as `zh-CN`, `en-US`. This texture will be selected when the game language is switched to the corresponding code.</li><li>`censored`: This texture will be selected when the "Blood and Gore" option is turned off.</li></ul> |
| `filter` | Integer | The filtering mode of the texture, defaulting to 1.<ul><li>`0`: Nearest, makes the texture more pixelated with stronger jagged edges.</li><li>`1`: Bilinear, makes the texture smoother and eliminates jagged edges.</li><li>`2`: Trilinear, makes the texture smoother, eliminates jagged edges, and blends between mipmap levels.</li></ul> |
| `wrap` | Integer | The repetition mode outside the texture, defaulting to 1.<ul><li>`0`: Repeat. If the rendering area is outside the texture, the texture will repeat outside the area.</li><li>`1`: Clamp. If the rendering area is outside the texture, the part outside the texture will use the color of the outermost edge.</li><li>`2`: Mirror. If the rendering area is outside the texture, the texture will be mirrored and repeated outside the area.</li><li>`3`: Mirror Once. If the rendering area is outside the texture, the texture will be mirrored and repeated once outside the area, then use the color of the outermost edge.</li></ul> |
| `slices` | List | A list of Sprite slices for the texture. |

Under `slices` is another list, where each element represents an in-game Sprite.

Basic properties of a slice include:
| Property | Type | Description |
| --- | --- | --- |
| `key` | String | The key of the Sprite to override. After adding this property, the `key` property in `texture` will be overridden by this value. Otherwise, the `key` property in `texture` will be used and appended with the slice index.<br>Suppose there is a texture path in the `resources` folder as `resources/assets/mvz2/sprites/ui/level/card_mobile.png`, and the key of the whole texture is `mvz2:ui/level/card_mobile`, but this texture is cut into multiple Sprite slices. In this case: <ul><li>The key of the first Sprite is `mvz2:ui/level/card_mobile[0]` (or `mvz2:ui/level/card_mobile`, where 0 can be omitted).</li><li>The key of the second Sprite is `mvz2:ui/level/card_mobile[1]`.</li><li>And so on.</li></ul>You can use `@` seperator to add tags for this key, such as `mvz2:ui/level/card_mobile[0]@en-US`. Valid tags are as follows:<ul><li>Language Code: Such as `zh-CN`, `en-US`. This texture will be selected when the game language is switched to the corresponding code.</li><li>`censored`: This texture will be selected when the "Blood and Gore" option is turned off.</li></ul> |
| `fullRect` | Boolean | The mesh type used when rendering this slice. Defaults to false. If true, the entire image, including transparent parts, will be counted as the rendering area. This might cause some rendering performance issues, but it needs to be enabled in certain cases (such as character textures). |

Positioning properties of a slice include:
| Property | Type | Description |
| --- | --- | --- |
| `single` | Boolean | Whether to directly use the entire texture file as the Sprite. Setting it to true will ignore the `x`, `y`, `width`, and `height` properties. |
| `x` | Integer | The starting horizontal coordinate of this slice in the texture. The leftmost side is 0, and each pixel to the right adds 1. Defaults to 0. |
| `y` | Integer | The starting vertical coordinate of this slice in the texture. The topmost side is 0, and each pixel downwards adds 1. Defaults to 0. |
| `width` | Integer | The width of this slice, in pixels. |
| `height` | Integer | The height of this slice, in pixels. |
| `pivotX` | Float | The horizontal pivot point of this slice, ranging from 0 to 1, where 0 is the left side and 1 is the right side. Defaults to 0.5, which is the exact center. |
| `pivotY` | Float | The vertical pivot point of this slice, ranging from 0 to 1, where 0 is the top side and 1 is the bottom side. Defaults to 0.5, which is the exact center. |

Sometimes, in-game textures need to be used as progress bars. In such cases, the texture needs to be sliced into a 9-patch image to ensure that the texture stretches correctly without errors. These types of textures include health bars, UI frames, etc.

The properties related to 9-patch images for a slice are as follows:
| Property | Type | Description |
| --- | --- | --- |
| `borderLeft` | Integer | When this slice is used as a 9-patch image, the distance between the center area and the leftmost edge. Defaults to 0. |
| `borderBottom` | Integer | When this slice is used as a 9-patch image, the distance between the center area and the bottommost edge. Defaults to 0. |
| `borderRight` | Integer | When this slice is used as a 9-patch image, the distance between the center area and the rightmost edge. Defaults to 0. |
| `borderTop` | Integer | When this slice is used as a 9-patch image, the distance between the center area and the topmost edge. Defaults to 0. |


### localization.json

An example of the content structure of `localization.json` is as follows:

    {
        "languages": [
            {
                "code": "en-US",
                "catalogs": [
                    "mvz2/catalogs/en-US/almanac.mo",
                    "mvz2/catalogs/en-US/general.mo",
                    "mvz2/catalogs/en-US/talk.mo"
                ]
            }
        ]
    }

Properties include:
| Property | Type | Description |
| --- | --- | --- |
| `languages` | List | A list of languages. |

Under `languages` is a list, where each element represents individual language information.
| Property | Type | Description |
| --- | --- | --- |
| `code` | String | Language code. |
| `catalogs` | String List | The paths of all `*.mo` files relative to the `assets` folder. |

#### Text Translation
To translate the text in the game, you need to use translation files in the [GNU Gettext](https://www.gnu.org/software/gettext/) format.

There are three file types for this translation format:
* `*.pot`: Translation text template, containing only source text.
* `*.po`: The text file translated into a specific language based on the translation template file.
* `*.mo`: The binary file generated by compiling the `*.po` file.

MVZ2 uses `*.mo` files for text translation. To create a `*.mo` file, you can use third-party tools such as [Poedit](https://poedit.net/).

The general process is:
- Create a new `*.po` file to translate the text.
- Load the translation template from a `*.pot` file into the `*.po` file.
- Perform text translation.
- Save the `*.po` file and compile it into a `*.mo` file.
- Place the compiled `*.mo` file anywhere under the `assets` directory.
- Add a reference to the `*.mo` file in `localization.json`.

#### Adding New Languages

To add a new language to the game, simply add a language with a specific language code to the `localization.json` file.

To display the custom language name properly, you can translate a text entry in the `*.po` file whose Context is `language_name`, representing the name of the current language.

### audios.json

An example of the content structure of `audios.json` is as follows:

    {
        "samples":[
            { 
                "id": "mvz2:click_1",
                "path": "click_1.wav"
            },
            { 
                "id": "mvz2:click_2",
                "path": "click_2.wav"
            },
            { 
                "id": "mvz2:click_3",
                "path": "click_3.wav"
            },
            { 
                "id": "mvz2:test_music",
                "path": "test_music.wav",
                "streaming": true
            },
            { 
                "id": "mvz2:test_music_sub",
                "path": "test_music_sub.wav",
                "streaming": true
            }
        ],
        "sounds":[
            { 
                "id": "mvz2:click",
                "samples": [
                    {
                        "id": "mvz2:click_1"
                    },
                    {
                        "id": "mvz2:click_2"
                    },
                    {
                        "id": "mvz2:click_3"
                    }
                ]
            }
        ],
        "musictracks":[
            { 
                "id": "mvz2:palace_level",
                "mainTrack": "mvz2:test_music",
                "subTrack": "mvz2:test_music_sub"
            }
        ]
    }

Properties include:
| Property | Type | Description |
| --- | --- | --- |
| `samples` | Array | A list of audio file samples. |
| `sounds` | Array | A list of sound effects. |
| `musictracks` | Array | A list of music tracks. |

Properties of elements under `samples` include:
| Property | Type | Description |
| --- | --- | --- |
| `id` | String | Audio ID. It can be a custom ID or the ID of an in-game audio sample to be replaced. Specific IDs can be checked in the `resources/audios.json` file of this project. |
| `path` | String | The path of the sound effect file relative to the `assets` folder. |
| `streaming` | Boolean | Whether to play via streaming, defaulting to false. For large audio files (such as music), reading them completely into memory may cause a momentary stutter. Setting it to true changes it to streaming playback, effectively mitigating this issue. |

Properties of elements under `sounds` include:
| Property | Type | Description |
| --- | --- | --- |
| `id` | String | The ID of the in-game sound effect to be replaced. Specific IDs can be checked in the `resources/audios.json` file of this project. |
| `samples` | List | A list of audio samples used by the sound effect. |
| `maxCount` | Integer | The maximum number of this sound effect that can be played simultaneously. Defaults to 2. |
| `priority` | Integer | The priority of the sound effect. The higher the number, the easier it is to be overridden and muted by other sound effects in the game. The range is 0-256, defaulting to 128. |
| `loopPitchStart` | Float | The pitch when the sound effect starts looping. Defaults to 1. |
| `loopPitchEnd` | Float | The pitch when the sound effect stops looping. Defaults to 1. |
| `loopVolume` | Float | The volume when the sound effect loops. Defaults to 1. |
| `loopFadeInSpeed` | Float | The speed at which the volume fades in when the sound effect starts looping. Defaults to 1. |
| `loopFadeOutSpeed` | Float | The speed at which the volume fades out when the sound effect stops looping. Defaults to 1. |

Properties of elements under the nested `samples` list mentioned above include:
| Property | Type | Description |
| --- | --- | --- |
| `id` | String | Audio sample ID. |
| `weight` | Float | Weight. The higher the weight, the more likely this audio sample will be randomly played. Defaults to 1. |

Properties of elements under `musictracks` include:
| Property | Type | Description |
| --- | --- | --- |
| `id` | String | The ID of the in-game music to be replaced. Specific IDs can be checked in the `resources/audios.json` file of this project. |
| `mainTrack` | String | Main audio sample ID. |
| `subTrack` | String | Optional. Sub audio sample ID. |

### fragments.json

    {
        "fragments": [
            {
                "id": "mvz2:dispenser",
                "colorKeys": [
                    {
                        "code": "4F4F4F",
                        "time": 0.349996179
                    },
                    {
                        "code": "ABABAB",
                        "time": 0.7000076
                    },
                    {
                        "code": "C3A26D",
                        "time": 1.0
                    }
                ],
                "alphaKeys": [
                    {
                        "alpha": 1.0,
                        "time": 0.0
                    },
                    {
                        "alpha": 1.0,
                        "time": 1.0
                    }
                ]
            }
        ]
    }

Properties include:
| Property | Type | Description |
| --- | --- | --- |
| `fragments` | List | A list of fragment particles. |

Properties of elements under `fragments` include:
| Property | Type | Description |
| --- | --- | --- |
| `id` | String | The ID of the in-game fragment particle to be replaced. Specific IDs can be checked in the `resources/fragments.json` file of this project. |
| `blend` | Boolean | Whether to enable gradient transitions between colors. Defaults to false. |
| `colorKeys` | `colorKey` List | A list of color keyframes. |
| `alphaKeys` | `alphaKey` List | A list of opacity keyframes. |

Properties of `colorKey` include:
| Property | Type | Description |
| --- | --- | --- |
| `code` | String | Color hex code. |
| `time` | Float | The position of this keyframe within the entire color table, which represents the proportion of this color. The range is 0-1. |

Properties of `alphaKey` include:
| Property | Type | Description |
| --- | --- | --- |
| `alpha` | Float | Opacity, ranging from 0 to 1, where 0 is fully transparent and 1 is fully opaque. |
| `time` | Float | The position of this keyframe within the entire color table, which represents the proportion of this opacity. The range is 0-1. |

A Fragment is a color table. The game will randomly pick a color from this color table and assign it to the fragment particles in the game.

### characters.json

An example of the content structure of `characters.json` is as follows:

    {
        "characters": [
            {
                "id": "mvz2:orant",
                "faceRight": false,
                "variants": [
                    {
                        "id": "mvz2:normal",
                        "width": 418,
                        "height": 1280,
                        "pivotX": 0.5,
                        "pivotY": 0.6,
                        "extendLeft": 0.0,
                        "extendRight": 0.0,
                        "layers": [
                            {
                                "sprite": "mvz2:characters/orant"
                            }
                        ]
                    },
                    {
                        "id": "mvz2:close_eye",
                        "width": 418,
                        "height": 1280,
                        "pivotX": 0.5,
                        "pivotY": 0.6,
                        "extendLeft": 0.0,
                        "extendRight": 0.0,
                        "layers": [
                            {
                                "sprite": "mvz2:characters/orant"
                            },
                            {
                                "sprite": "mvz2:characters/orant[1]"
                            }
                        ]
                    },
                    {
                        "id": "mvz2:warning",
                        "width": 418,
                        "height": 1280,
                        "pivotX": 0.5,
                        "pivotY": 0.6,
                        "extendLeft": 0.0,
                        "extendRight": 0.0,
                        "layers": [
                            {
                                "sprite": "mvz2:characters/orant"
                            },
                            {
                                "sprite": "mvz2:characters/orant[2]"
                            }
                        ]
                    }
                ]
            }
        ]
    }

**Note: The usage of `characters.json` is different from other content manifests. Other content manifests directly replace the contents of the original game, whereas `characters.json` modifies individual entries on top of the original content.**

Properties include:
| Property | Type | Description |
| --- | --- | --- |
| `characters` | List | A list of characters. |

Properties of elements under `characters` include:
| Property | Type | Description |
| --- | --- | --- |
| `id` | String | The ID of the in-game character to be modified. Specific IDs can be checked in the `resources/characters.json` file in this project. |
| `faceRight` | Boolean | Whether the character faces right by default. Default value is false. |
| `variants` | `variant` List | A list of variants for this character. |

Properties of elements under `variants` include:
| Property | Type | Description |
| --- | --- | --- |
| `id` | String | The ID of the in-game character variant to be modified. Specific IDs can be checked in the `resources/characters.json` file in this project. |
| `width` | Integer | The width of the variant to be modified, in pixels. If this property is not added, it will not be modified. |
| `height` | Integer | The height of the variant to be modified, in pixels. If this property is not added, it will not be modified. |
| `pivotX` | Float | The horizontal pivot point of the variant to be modified, ranging from 0 to 1, where 0 is the left side and 1 is the right side. If this property is not added, it will not be modified. |
| `pivotY` | Float | The vertical pivot point of the variant to be modified, ranging from 0 to 1, where 0 is the top side and 1 is the bottom side. If this property is not added, it will not be modified. |
| `extendLeft` | Float | The left extension value of the variant to be modified, used to scale up or down the size of the character in dialogue. If this property is not added, it will not be modified. |
| `extendRight` | Float | The right extension value of the variant to be modified, used to scale up or down the size of the character in dialogue. If this property is not added, it will not be modified. |
| `layers` | List | The layer array of the variant to be replaced. Note that this list will directly replace all layers of the variant. If this property is not added, it will not be overwritten. |

Properties of elements under `layers` include:
| Property | Type | Description |
| --- | --- | --- |
| `sprite` | String | The texture key of this character layer. |
| `x` | Float | The horizontal position of this layer within the entire character. 0 is the leftmost side, and each pixel to the right adds 1. |
| `y` | Float | The vertical position of this layer within the entire character. 0 is the topmost side, and each pixel downwards adds 1. |

# Application

On the Windows platform, the resource pack directory is `%HOMEPATH%\AppData\LocalLow\Cuerzor\MinecraftVSZombies2\resource_packs`. Simply place the resource pack folder or the compressed zip file into this directory to use it.

You can also apply the resource pack via the in-game import operation.