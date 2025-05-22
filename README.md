# CWC_PalletDrag

## A Custom Web Control (CWC) to drag boxes on a pallet in WinCC Unified

![A pile of stacked pallets]({8d4fed09-7c4a-4af1-85cb-1d7c24c6b9a7}/Utils/palletsWhite.png)

Based on [anseki's plain-draggable.js library](https://github.com/anseki/plain-draggable)

## ToDo:

- [x] Get requirements
- [x] Find a good idea
- [x] Find a good js library
- [x] Write code
- [x] Debug
- [x] Write a meaningful documentation
- [ ] Further improvements

## How to create a CWC from this stuff:

1. Download/pull/whatever the code
2. cd in the **{8d4fed09-7c4a-4af1-85cb-1d7c24c6b9a7}** folder
3. Create **{8d4fed09-7c4a-4af1-85cb-1d7c24c6b9a7}.zip** file with the **{8d4fed09-7c4a-4af1-85cb-1d7c24c6b9a7}** folder content
> [!IMPORTANT]
> Do not include the **{8d4fed09-7c4a-4af1-85cb-1d7c24c6b9a7}** folder itself as root folder in the file zip!
4. Copy the **{8d4fed09-7c4a-4af1-85cb-1d7c24c6b9a7}.zip** file in **C:\Program Files\Siemens\Automation\Portal Vxx\Data\Hmi\CustomControls** folder
5. (Re)start TIA Portal
6. Drag your new CWC from the **My Controls** right side pane in a WinCC Unified screen
7. Enjoy

## How to directly download this CWC (if you are in a hurry):
1. Download from [here](Build/{8d4fed09-7c4a-4af1-85cb-1d7c24c6b9a7}.zip?raw=true)
2. Copy the **{8d4fed09-7c4a-4af1-85cb-1d7c24c6b9a7}.zip** file in **C:\Program Files\Siemens\Automation\Portal Vxx\Data\Hmi\CustomControls** folder
3. (Re)start TIA Portal
4. Drag your new CWC from the **My Controls** right side pane in a WinCC Unified screen
5. Enjoy

## Documentation:

This CWC (Custom Web Control) is intended to generate an array of coordinates leveraging a simplified interaction method with the operator (drag&drop).

This happens making the CWC generate events on interaction and react to methods called by WinCC Unified.

### The "box":
The data exchanged between the CWC and WinCC Unified is based on the "box" object:

```
{
    'id': 'box0', //an alphanumeric Id of the box. Optional: if empty a "boxN" value is created
    'sizeX': 150, //size of the box on X axis in pixels
    'sizeY': 100, //size of the box on Y axis in pixels
    'sizeZ': 100, //size of the box on Z axis in pixels
    'posX': 0, //position of the center of the box on X axis in pixels (0 represents the mean point on the playground)
    'posY': 50, //position of the center of the box on Y axis in pixels (0 represents the mean point on the playground). Optional: if empty, sizeY/2 is taken
    'posZ': 0, //position of the center of the box on Z axis in pixels (0 represents the mean point on the playground)
    'color': '0x009999' //HTML code color of the box. Optional: if empty, a random color is selected
}
```

> [!IMPORTANT]
> The 2D coordinates of the playground are X (horizontal axis) and Z (vertical axis). Y is currently not used (future 3D implementations).

### Data exchange
Here you have a short description of events, methods and parameters supported by this CWC.

#### Events:
- **`BoxSelected`**. This event is fired when a box is selected in the playground and delivers a string argument: **`SelectedBoxId`**. This represents the alphanumeric Id of the box which has been just selected.
- **`BoxMoving`**. This event is countinously fired in order to deliver a realtime information about the position of a box which is selected and dragged, just during the movement. This is done delivering the argument **`MovingBox`**, a JSON string which the typical box structure.
- **`BoxReleased`**. This event is fired when a box is released, even if not moved. The event deliver the complete array of box JSON structure currently in the playground.
- **`BoxCreated`**. This event is raised as soon as a new box is added to the playground, typically responding to the **`AddBox`** method. The argument delivered is the new box JSON structure.
- **`BoxDeleted`**. This event is raised as soon as a new box is removed from the playground, typically responding to the **`DelBox`** method. The argument delivered is the removed box Id.

#### Methods:
- **`AddBox`**. This method causes a new box to be created in the playground. It takes a JSON box structure as argument. Some fields are optional **`(Id, color, PosY)`**
- **`DelBox`**. This method causes a box to be removed from the playground. It takes a box Id as single parameter, which can be omitted. In this case, the last selected box, if any, is removed.
- **`Reset`**. This method has no arguments and just causes the CWC to restart from the **`InitialBoxesConfig`** box and **`PlayGroundConfig`** configurations parameters.

#### Parameters:
This CWC has accepts 2 static parameters, which represent the initial configuration of the CWC:
- **`PlayGroundConfig`**. This parameter is a JSON string with the playground size in pixels: **`{'sizeX': 800, 'sizeZ': 600}`**.
- **`InitialBoxesConfig`**. This parameter is a JSON array of box structures, which represents the initial configuration of the playground in term of boxes.

## TIA Portal example project:
You can download a TIA Portal V20 Update 3 example project [here](Demo/TestPallet2D_20250522_1519.zap20?raw=true)

## Disclaimer
The code provided in this repository is intended as an example only. We do not guarantee that the code is error-free or suitable for any specific purpose. Use of the code is at the user's discretion. We do not assume any responsibility for any issues arising from the use of the code. 

The copyright of any libraries included in the code belongs to their respective developers.
