# WorldUI Documentation

## Description

WorldUI (WUI) is a tool to create your own styled UI menu for your VRChat world.
A default style is included, but is fully customizable by any and all creators.
This system has two primary prefabs to make use of:

-   **World Menu** (a menu placable menu for your world)
-   **Menu Placement Region** (move the menu to a given position when a player walks into a region)

The menu has a three tier structure: **Tabs > Pages > Elements**.

Each tab contains a list of pages, while each page contains a list of elements.
Elements can be anything from a simple label (see "Builder Label") to a Single Select toggle for GameObjects (see "Builder Single Select Toggle") in your scene.

## Instructions

To make your own menu, use the prefab within `Assets / Vowgan / WorldUI / Prefabs / World Menu.prefab`

Placing this in your scene will let you begin customizing your menu.
Begin by adding a new Tab, assign an Title and Icon, and create a page.
Assign a Title and Icon to the page, and add a new element.
Select the element you want to use, and set any of the variables needed.

## Elements

### Base Types

Note: Fields marked as **"Binding Only"** are intended to be manually integrated with other scripts and components.
All others can be easily used by world creators.

| Element       | Description                                                                                                       |
| ------------- | ----------------------------------------------------------------------------------------------------------------- |
| Dropdown      | **Binding Only** - Provide a list of options used within the dropdown.                                            |
| Event         | Run an event after pushing a button. Provide a list of "Events" that are called when pressed.                     |
| Label         | Display an Icon, Title, and Description with no background.                                                       |
| Single Select | **Binding Only** - (Also known as "Radio Button") Provide a list of entries within the selection.                 |
| Slider        | **Binding Only** - Contains a MinMax range (0-1), a MinMax display range (0-100), and a string as the suffix (%). |
| Teleport      | Teleport the player to a target Transform. Uses the WuiTeleportModule.                                            |
| Toggle        | **Binding Only** - Toggles a bool On and Off.                                                                     |

### Variants

| Element                | Description                                                                                   |
| ---------------------- | --------------------------------------------------------------------------------------------- |
| Single Select Quality  | A SingleSelect for controlling the Quality settings for the world. Uses the WuiQualityModule. |
| Single Select Toggle   | Toggle on a single GameObject from a list.                                                    |
| Slider Post Processing | Control the intensity of a list of PostProcessing Volumes.                                    |
| Slider Volume          | Control the volume of a list of AudioSources.                                                 |
| Toggle Objects         | Toggle On and Off a list of GameObjects.                                                      |

## Modules

WorldUI comes with a system for implementing "Modules".
You can implement your own by inheriting from "WuiModuleBase" from the "Vowgan.WorldUI.Modules" namespace.

Two modules are provided by this system:

| Module              | Description                                                                    |
| ------------------- | ------------------------------------------------------------------------------ |
| Wui Quality Module  | Control different quality features of the world between High, Medium, and Low. |
| Wui Teleport Module | Fully-featured teleport system with transition effects.                        |

## Attributes

There's some custom attributes implemented into this project you can make use of.

| Attributes       | Description                                                                                                                                     |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| FindObject       | Runs "FindObjectOfType" during PostProcessScene. Field must be Serialized.                                                                      |
| FindParent       | Runs "GetComponent" on the instance's Parent transform during PostProcessScene. Field must be Serialized.                                       |
| FindObjectArray  | Runs "FindObjectsOfType" during PostProcessScene. Field must be an array and Serialized.                                                        |
| FindSiblings     | Runs "GetComponentsInChildren" on the instance's Parent transform during PostProcessScene. Field must be an array and Serialized.               |
| FindChildren     | Runs "GetComponentsInChildren" during PostProcessScene. Field must be an array and Serialized.                                                  |
| SubclassDropdown | Display a dropdown of all non-abstract subclasses of the given field. This must be used in conjunction with the 'SerializeReference' attribute. |
| Vector2Label     | Enforce custom labels for the X and Y values on a Vector2 field.                                                                                |

## Tweening

UI Animations are handled via a custom "WuiTween" system.
This is a custom tweening implementation built for Udon, with a maximum active tween count of 100.
WuiTween features require at least one "WuiTween" component existing in the scene.

The standard way of utilizing WuiTween is to use a WuiTweenAction component.
This gives a list of TweenActions that will be run either when the GameObject is enabled, or when "\_RunTween" is called on the backing WuiTweenActionUdon behaviour.

You can directly interface with WuiTween by calling different methods:

-   \_TweenPos: Tween a transform's position.
-   \_TweenPosX, Y, Z: Tween a transform's X, Y, or Z position.
-   \_TweenLocalPos: Tween a transform's local position.
-   \_TweenLocalPosX, Y, Z: Tween a transform's X, Y, or Z local position.
-   \_TweenRot: Tween a transform's rotation.
-   \_TweenLocalRot: Tween a transform's local rotation.
-   \_TweenScale: Tween a transform's scale, either via Vector3 or Float.
-   \_TweenColor: Tween an Image component's color.
-   \_TweenOpacity: Tween a CanvasGroup's opacity.
-   \_TweenWidth: Tween a LayoutElement's width.
-   \_TweenHeight: Tween a LayoutElement's height.
-   \_TweenAnchorPosition: Tween a RectTransform's Anchor Position.

Each of these return an index of the active tween task.
If -1 is returned, the tween was not started.

Since WuiTween doesn't apply a start value, a few extension methods have been provided for a transform:

-   SetPositionX, Y, Z: Set specifically one value in the position vector.
-   SetLocalPositionX, Y, Z: Set specifically one value in the local position vector.

## Styling

Styling in WorldUI is tiered into three separate parts:

-   UI Style Settings: Storage for the currently active visual styling.
-   UI Style Palette: A selection of pre-existing colors and themes.
-   UI Style Library: Specific rules for colors and styles when used on in-scene components.

The Settings aren't necessary for the end-user, but are accessible through the `ProjectSettings > WorldUI`.
The Palette has various colors predefined for most UI needs.

The Style Library is a bit more complicated. This has "Base Styles" and "Compound Styles", which are built out of the Base Styles.
Image Styles contain a color, a sprite, and the PizelsPerUnitMultiplier.
If a sprite is provided, this style will overwrite an existing one on the component, but will use the existing one if none is provided on the style.
Label Styles contain a Color, a TextMeshPro Font Asset, and a Font Size.
Selectable Styles control what happens when a "Selectable" (such as a Button, a Dropdown, a Slider, etc.) is hovered over, clicked, and more.

Compound styles are a combination of the Image, Label, and Selectable styles.
These are used for Buttons, Toggles, Sliders, Scrollbars, and Dropdowns.
Those are selectable from a dropdown on their corrosponding components:

-   UI Style Button
-   UI Style Dropdown
-   UI Style Image
-   UI Style Label
-   UI Style Scrollbar
-   UI Style Slider
-   UI Style Toggle

Custom implementations can be made by inheriting from "BaseUiStyleComponent" from the "Vowgan.WorldUI.Styling" namespace.
