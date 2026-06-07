语言：[English](README.md) | 简体中文

这篇文章介绍了如何创建一个MVZ2的资源包。

你可以下载本仓库，其中的一些文件夹提供了一些必要的资源：

- `examples`：资源包示例，你可以仿照这些示例进行资源包的开发，并对其文件结构进行研究学习。
- `templates`：必要的贴图模板，翻译文本模板，以及用作参考的英文翻译文本。
- `resources`：原版游戏的所有贴图和音频资源。

# 信息文件
信息文件位于资源包的根目录下，文件名为pack.json。

其内容格式一般为：

    {
        "name": "语言包名称",
        "author": "作者名称",
        "description": "语言包描述",
        "dataVersion": 0,
        "version": {
            "major": 0,
            "minor": 0
        }
    }

其中：

| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `name` | 字符串 | 该资源包的名称，用于在资源包界面显示。|
| `author` | 字符串 | 该资源包的作者名称，暂时无作用。|
| `description` | 字符串 | 该资源包的简短描述，用于在资源包界面显示。|
| `dataVersion` | 整数 | 该资源包的数据版本，用于在不同版本的游戏之间进行格式检查。<ul><li>目前原版游戏的数据版本为1。</li><li>如果`dataVersion`低于这个值，那么会提示该资源包为过时资源包，但仍会尝试加载。</li><li>如果`dataVersion`高于这个值，那么会提示该资源包为不兼容的资源包，且不会尝试加载。</li></ul>|
| `version` | object | 该资源包的版本号，暂时无作用。|

**一个语言包必须存在信息文件才能够被游戏识别，没有信息文件的语言包是无效的。**

# 图标
图标文件位于资源包的根目录下，文件名为pack.png。

建议使用正方形的贴图作为语言包的图标。

# 文件结构
除了作为信息文件的pack.json和作为图标的pack.png，资源包的内容清单放在根目录下，具体内容则放在assets文件夹中。

## 内容清单

一个资源包中可以包含多种内容清单，它们都需要放在根目录下，包括：
- `textures.json`：用于管理贴图。
- `localization.json`：用于管理本地化文本文件。
- `audios.json`：用于管理音效和音乐。
- `fragments.json`：用于管理器械和一些其他实体受伤时掉落的粒子颜色。
- `characters.json`：用于管理对话中的角色。

### textures.json

textures.json的内容结构示例如下：

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

属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `textures` | 列表 | 贴图列表。 |

`textures`之下是一个列表，里面的每个元素都是一张单独的贴图文件。属性包括：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `texture` | 字符串 | 贴图相对于`assets`文件夹的文件路径。如`assets/card_mobile.png`，在这里需要写为`card_mobile.png`。 |
| `key` | 字符串 | 要替换的游戏内Sprite的键。Sprite的具体键可以查看该项目中`resources/textures.json`文件。<br>该属性会用于该贴图的所有切片`slices`中，并自动为他们分配键。除非切片本身实现了`key`属性。<br>可以通过`@`分隔符来为键添加标签，如`mvz2:ui/level/card_mobile@en-US`。有效标签如下：<ul><li>语言代码：如`zh-CN`，`en-US`。将游戏中的语言切换为对应代码的语言时，会选用该贴图。</li><li>`censored`：在关闭选项“血与碎块”时，会选用该贴图。</li></ul> |
| `filter` | 整数 | 贴图的过滤模式，默认为1。<ul><li>`0`：邻近，会使贴图变得更加像素化，锯齿感更强。</li><li>`1`：双线性，会使贴图变得较为模糊，并消除锯齿感。</li><li>`2`：三线性，会使贴图变得较为模糊，并消除锯齿感，并在mipmap级别之间进行混合。</li></ul> |
| `wrap` | 整数 | 贴图外部的重复模式，默认为1。<ul><li>0：重复。如果渲染区域位于贴图外部，那么贴图会在区域外重复。</li><li>`1`：钳制。如果渲染区域位于贴图外部，那么贴图外部的部分会使用最边缘的颜色。</li><li>`2`：镜像。如果渲染区域位于贴图外部，那么贴图会在区域外镜像重复。</li><li>`3`：镜像一次。如果渲染区域位于贴图外部，那么贴图会在区域外镜像重复一次，然后使用最边缘的颜色。</li></ul> |
| `slices` | 列表 | 贴图的Sprite切片列表。 |

`slices`之下又是一个列表，里面的每个元素都是一张游戏内的Sprite。

slice的基础属性包括：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `key` | 字符串 | 要覆盖的Sprite的键。添加该属性后，`texture`中的`key`属性会被覆盖为该值，否则会自动使用`texture`中的`key`属性，并加上切片序号。<br>假设`resources`文件夹中有一张贴图的路径为`resources/assets/mvz2/sprites/ui/level/card_mobile.png`，则整张贴图的键是`mvz2:ui/level/card_mobile`，但是这张贴图又被切为了多张Sprite切片，此时：<ul><li>第一张Sprite的键为`mvz2:ui/level/card_mobile[0]`（或者`mvz2:ui/level/card_mobile`，0可以省略）。</li><li>第二张Sprite的键为`mvz2:ui/level/card_mobile[1]`。</li><li>以此类推。</li></ul>可以通过`@`分隔符来为键添加标签，如`mvz2:ui/level/card_mobile[0]@en-US`。有效标签如下：<ul><li>语言代码：如`zh-CN`，`en-US`。将游戏中的语言切换为对应代码的语言时，会选用该Sprite。</li><li>`censored`：在关闭选项“血与碎块”时，会选用该Sprite。</li></ul> |
| `fullRect` | 布尔值 | 这张切片的在渲染时使用的网格类型。默认为false。如果为true，那么整张图，包括透明部分也会被算作渲染区域。这可能会带来一些渲染性能的问题，不过在某些情况下（如作为人物贴图）时需要开启。 |

slice的定位属性包括：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `single` | 布尔值 | 是否直接使用整张贴图文件作为Sprite。设为true则会忽略`x`，`y`，`width`和`height`属性。 |
| `x` | 整数 | 这张切片在贴图中的起始横向坐标。最左侧为0，每向右1像素+1。默认为0。 |
| `y` | 整数 | 这张切片在贴图中的起始纵向坐标。最顶部为0，每向下1像素+1。默认为0。 |
| `width` | 整数 | 这张切片的宽度，单位为像素。 |
| `height` | 整数 | 这张切片的高度，单位为像素。 |
| `pivotX` | 浮点数 | 这张切片的横向枢轴点，范围为0-1，0为左侧，1为右侧。默认为0.5，即最中间。 |
| `pivotY` | 浮点数 | 这张切片的纵向枢轴点，范围为0-1，0为顶部，1为底部。默认为0.5，即最中间。 |

有的时候，游戏内的贴图需要用作进度条，这个时候需要将贴图切分为九宫格图保证贴图在拉伸的时候不会出现错误。这类贴图有血条、UI框等。

slice的九宫格图相关属性如下：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `borderLeft` | 整数 | 这张切片作为九宫格图时，中间区域与最左侧的距离。默认为0。 |
| `borderBottom` | 整数 | 这张切片作为九宫格图时，中间区域与最底部的距离。默认为0。 |
| `borderRight` | 整数 | 这张切片作为九宫格图时，中间区域与最右侧的距离。默认为0。 |
| `borderTop` | 整数 | 这张切片作为九宫格图时，中间区域与最顶部的距离。默认为0。 |


### localization.json

localization.json的内容结构示例如下：

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

属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `languages` | 列表 | 语言列表。 |

`languages`之下是一个列表，里面的每个元素都是一个单独的语言信息。
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `code` | 字符串 | 语言代码。 |
| `catalogs` | 字符串列表 | 所有`*.mo`文件相对于`assets`文件夹的路径。 |

#### 文本翻译
要翻译游戏中的文本，你需要用到[GNU Gettext](https://www.gnu.org/software/gettext/)格式的翻译文件。

这种翻译格式有三种文件类型：
* \*.pot：翻译文本模板，只有源文本。
* \*.po：根据翻译文件模板，针对某种语言进行翻译后的文本文件。
* \*.mo：将\*.po文件编译后产生的二进制文件。

MVZ2使用\*.mo文件进行文本翻译。要想创建一个\*.mo文件，你可以使用一些第三方工具，如[Poedit](https://poedit.net/)。

大体流程为：
- 新建`*.po`文件用于翻译文本
- 从一个`*.pot`文件中加载翻译模板到`*.po`文件中。
- 进行文本翻译。
- 保存`*.po`文件，并将其编译为`*.mo`文件。
- 将编译过后的`*.mo`放置于`assets`目录下的任意位置。
- 在`localization.json`中添加对该`*.mo`的引用。

#### 添加新语言

如果要向游戏添加新的语言，在`localization.json`文件中添加一个具有特定语言代码的语言即可。

要想使自定义的语言的名称能够正常显示，可以在`*.po`文件中翻译一项文本，这项文本的上下文（Context）为`language_name`，用于代表当前语言的名称。

### audios.json

audios.json的内容结构示例如下：

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

属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `samples` | 数组 | 音频文件采样列表。 |
| `sounds` | 数组 | 音效列表。 |
| `musictracks` | 数组 | 音乐列表。 |

`samples`的元素的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `id` | 字符串 | 音频ID。可以是自己设置的ID，也可以是要替换的游戏内音频采样的ID。具体ID可以查看该项目中`resources/audios.json`文件。 |
| `path` | 字符串 | 音效文件相对于`assets`文件夹的路径。 |
| `streaming` | 布尔值 | 是否流式播放，默认为false。针对一些体积比较大的音频（如音乐），直接全部读取到内存中会导致有一瞬间的卡顿。如果设置为true，那么会改为使用流式播放，有效缓解这一问题。 |

`sounds`的元素的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `id` | 字符串 | 要替换的游戏内音效的ID。具体ID可以查看该项目中`resources/audios.json`文件。 |
| `samples` | 列表 | 音效使用的音效采样列表。 |
| `maxCount` | 整数 | 同时最多播放多少个该音效。默认为2。 |
| `priority` | 整数 | 该音效的优先级，优先级的数字越高，在游戏中就越容易被其他音效覆盖而静音。范围为0-256，默认为128。 |
| `loopPitchStart` | 浮点数 | 音效开始循环播放时的音调。默认为1。 |
| `loopPitchEnd` | 浮点数 | 音效停止循环播放时的音调。默认为1。 |
| `loopVolume` | 浮点数 | 音效循环播放时的音量。默认为1。 |
| `loopFadeInSpeed` | 浮点数 | 音效开始循环播放时，音量渐入的速度。默认为1。 |
| `loopFadeOutSpeed` | 浮点数 | 音效结束循环播放时，音量渐出的速度。默认为1。 |

上面提到的`samples`的元素的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `id` | 字符串 | 音频采样ID。 |
| `weight` | 浮点数 | 权重。权重越高，该音频采样越容易被随机播放。默认为1。 |

`musictracks`的元素的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `id` | 字符串 | 要替换的游戏内音乐的ID。具体ID可以查看该项目中`resources/audios.json`文件。 |
| `mainTrack` | 字符串 | 主音频采样ID。 |
| `subTrack` | 字符串 | 可选。副音频采样ID。 |

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
            },
        ]
    }

属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `fragments` | 列表 | 碎片粒子列表。 |

`fragments`的元素的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `id` | 字符串 | 要替换的游戏内碎片粒子ID。具体ID可以查看该项目中`resources/fragments.json`文件。 |
| `blend` | 布尔值 | 是否在颜色间启用渐变。默认为false。 |
| `colorKeys` | `colorKey`列表 | 颜色帧列表。 |
| `alphaKeys` | `alphaKey`列表 | 透明度帧列表。 |

`colorKey`的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `code` | 字符串 | 颜色代码。 |
| `time` | 浮点数 | 该关键帧位于整个颜色表中的位置，可用于表示该颜色的占比。范围为0-1。 |

`alphaKey`的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `alpha` | 浮点数 | 透明度，范围为0-1，0为完全透明，1为完全不透明。 |
| `time` | 浮点数 | 该关键帧位于整个颜色表中的位置，可用于表示该透明度的占比。范围为0-1。 |

一个Fragment就是一个颜色表，游戏会从这个颜色表中随机取一个颜色，并将其赋给游戏中的碎片粒子。

### characters.json

`characters.json`的内容结构示例如下：

    [
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
    ]

**注意：`characters.json`的使用方式与其他内容清单不同。其他内容清单是直接替换原版游戏的内容，`characters.json`则是在原版内容的基础上进行单项修改。**

属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `characters` | 列表 | 人物列表。 |

`characters`的元素的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `id` | 字符串 | 要修改的游戏内人物的ID。具体ID可以查看该项目中的`resources/characters.json`文件。 |
| `faceRight` | 布尔值 | 该人物是否默认朝向右侧。默认值为false。 |
| `variants` | `variant`列表 | 该人物的变体列表。 |

`variants`的元素的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `id` | 字符串 | 要修改的游戏内人物变体的ID。具体ID可以查看该项目中的`resources/characters.json`文件。 |
| `width` | 整数 | 要修改的该变体的宽度，单位为像素。不添加该属性则不修改。 |
| `height` | 整数 | 要修改的该变体的高度，单位为像素。不添加该属性则不修改。 |
| `pivotX` | 浮点数 | 要修改的该变体的横向枢轴点，范围为0-1，0为左侧，1为右侧。不添加该属性则不修改。 |
| `pivotY` | 浮点数 | 要修改的该变体的纵向枢轴点，范围为0-1，0为顶部，1为底部。不添加该属性则不修改。 |
| `extendLeft` | 浮点数 | 要修改的该变体的左侧扩展值，用于放大或缩小对话中人物的大小。不添加该属性则不修改。 |
| `extendRight` | 浮点数 | 要修改的该变体的右侧扩展值，用于放大或缩小对话中人物的大小。不添加该属性则不修改。 |
| `layers` | 列表 | 要替换的该变体的图层数组，注意这个列表会直接替换该变体的所有图层。不添加该属性则不覆盖。 |

`layers`的元素的属性有：
| 属性 | 类型 | 说明 |
| --- | --- | --- |
| `sprite` | 字符串 | 该人物图层的贴图键。 |
| `x` | 浮点数 | 该图层在该整个人物中的横向位置。0为最左侧，每向右1像素+1。 |
| `y` | 浮点数 | 该图层在该整个人物中的纵向位置。0为最顶部，每向下1像素+1。 |

# 应用

在Windows平台下，资源包目录为`%HOMEPATH%\AppData\LocalLow\Cuerzor\MinecraftVSZombies2\resource_packs`。将语言包文件夹，或者压缩后的zip文件放入这个目录即可使用。

你也可以通过游戏内的导入操作来应用语言包。