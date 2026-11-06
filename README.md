# RigNode

Rigging workflow tools for Maya 2025 and 2026, in one Python file. It speeds up everyday rigging tasks, but it is not an auto-rigger.

Copyright (c) 2026 Enzo Freminet. All rights reserved.

## Install

1. Drag `rignode.py` from your file browser into the Maya viewport.
2. Maya copies the file to your user scripts folder (`Documents/maya/<version>/scripts`), adds a button with the RigNode logo to the current shelf, and opens the toolkit.

The logo is stored inside `rignode.py`, so you only need that one file. Install once for each Maya version.

**To update:** drop the new file, with no restart. The shelf button is replaced, not duplicated. If you have several Maya sessions open, each one keeps the code it loaded until you drop the file there or restart it. Versions before 1.7 ignore a newer file dropped on them, so to update from those, close every Maya session first.

**Coming from Rig Toolkit:** every time RigNode opens, it closes the old window and removes its script, icons and shelf button. If the old window opens empty when Maya starts, click the RigNode button or drop `rignode.py`, and it won't come back. A session that was already open keeps the old version until you do the same there.

## Launch

Click the RigNode button on the shelf, or run this in a Python tab of the Script Editor:

```python
import rignode; rignode.show()
```

Running it again replaces the open window. The toolkit only opens when you ask: if it was open when you quit Maya, it closes again at the next startup. The window can be docked anywhere in Maya. When it floats, its height adjusts to the current tab, so there is no empty space. When docked, it takes the size of the dock.

## How the window works

- The **top panel** is always visible and holds the tools you use most.
- The **five tabs** below it group everything else: Controls, Shapes, Rename, Transform and Rig.
- Hover over any button to see what it does. Small gray text under a heading tells you what to select.
- Every action can be undone in one Ctrl+Z. If the selection is wrong, a message appears in the viewport and nothing changes.

## Top panel

### Match

Select the driver first, then the targets. The targets move to the driver.

| Button | What it does |
|---|---|
| **T / R / S** checkboxes | Choose what **Match All** copies: translation, rotation and scale. |
| **Match All** | Matches the checked channels. |
| **Translate** | Matches position only. |
| **Rotate** | Matches rotation only. |

### Quick tools

| Button | What it does |
|---|---|
| **Freeze** | Freezes translate, rotate and scale on the selection. |
| **History** | Deletes construction history on the selection. |
| **Pivot** | Centers the pivot of the selected objects. |
| **LRA** | Toggles the local rotation axis display on the selected objects or joints. |
| **CVs** | Selects all CVs of the selected curves. |
| **Node Editor** | Opens the Node Editor. Right-click it to set its options, which stay after you restart Maya. **Don't add new nodes automatically** turns off Maya's **Add to Graph on Create**. **Don't follow the viewport selection** turns off Maya's **Sync Node Editor/Viewport Selection**. |
| **Joint** | Creates a joint named `JNT_<name>` on each selected object, or at the center of the selected vertices, edges or faces. With nothing selected, it creates one at the origin. New joints are not parented. |
| **Show/Hide** | Shows or hides all joints in every viewport. |
| **Joint size** slider | Sets the display size of all joints. |

## Controls tab

Creates NURBS controllers and colors them. Each controller is named `ctrl_<name>` and sits inside an offset group named `grp_offset_ctrl_<name>`, so its channels start at zero.

Clicking a shape creates the controller straight away. You get one per selected object, matched to its position and rotation, or one at the origin if nothing is selected. New controllers have a line width of 3 and are left selected, so you can click a color next.

### Options

| Option | What it does |
|---|---|
| **Name** | Base name of the controller. If blank, it uses the selected object's name without its `ctrl_`, `grp_offset_`, `JNT_`, `jnt_`, `geo_` or `loc_` prefix. With several objects selected, a number is added: `ctrl_arm_01`, `ctrl_arm_02`. |
| **Size** | Controller size in scene units. |
| **Axis** | The axis the controller faces. |

### Shape

Hover over a picture to see its name. The shapes are Circle, Square, Cube, Sphere, Diamond, Arrow, Double Arrow, Cross, Pin, Gear, Axis, Box, Cone, Cross Arrow, Curved Cross, Circle Arrows, Square Arrows, Aim, Rotate, Dial, Arcs, Pill, Switch, Eyes, Crescent, Star, Cylinder and Foot.

### Text

Type some text, then click **Create** or press Enter to draw it as an Arial curve controller. The letters are merged into one controller, centered, and as tall as **Size**. If **Name** is blank, the controller is named after the text: `IK` gives `ctrl_IK`.

### Color

Colors are set as overrides on the shape nodes of the selected controllers.

| Button | What it does |
|---|---|
| **Swatches** | Apply a Maya color index. |
| **Custom RGB...** | Opens a color picker and applies any color. |
| **Reset Color** | Removes the color override. |

## Shapes tab

### Line width

Sets the Line Width attribute of the selected curve shapes, which controls their thickness in the viewport.

| Button | What it does |
|---|---|
| **Apply** | Sets the entered value on the selected curves. |
| **Reset** | Goes back to Maya's default line width. |

### Mirror across X

Sides are found from `L_` / `R_` or `_L` / `_R` in the name.

| Button | What it does |
|---|---|
| **Mirror Shape** | Copies the shape of the selected controllers onto their existing other side, mirrored. The other side keeps its color. |
| **Mirror Controller** | Creates the other side of the selected controllers, with its offset group. It is skipped if the other side already exists. |
| **Behavior orientation** | On: new controllers are oriented like Mirror Joint with Behavior. Off: world-aligned controllers stay world-aligned, which suits IK feet, for example. |

### Copy and flip

| Button | What it does |
|---|---|
| **Copy Shape** | Select the source first, then the targets. Copies the shape, and each target keeps its color. |
| **Flip Direction** | Reverses the direction (normal) of the selected curves. |
| **Flip X / Y / Z** | Flips the shape across its local axis. The transform does not change. |

### Scale shape

Enter a value, then click **Bigger** to multiply the shape size by it, or **Smaller** to divide by it. Only the CVs move, so the transform does not change.

## Rename tab

Choose **Selected**, or **Selected + hierarchy** to include all children. Nodes are renamed from the deepest child up, so duplicate short names are safe. The final names are printed in the Script Editor.

| Tool | What it does |
|---|---|
| **Quick prefix** | Adds `ctrl_`, `grp_offset_`, `JNT_`, `geo_` or `loc_`. If the name already has it in another case, such as `jnt_`, only the case changes. |
| **Swap L / R** | Swaps `L_` and `R_`, or `_L` and `_R`. Swapping both sides at once is safe. |
| **Add text** | Adds a prefix or a suffix. |
| **Search and replace** | Replaces text in each name. Leave Replace blank to remove the text. |
| **Remove characters** | Removes a number of characters **From Start** or **From End**. |
| **Auto-number** | Numbers names in selection order. Set the base name (blank keeps the current names), the number of digits and the first number. |
| **Select by name** | Selects all objects whose name contains the text. `*` works as a wildcard. |

## Transform tab

### Pivot

Joints are skipped, because moving a joint's pivot breaks its transform.

| Button | What it does |
|---|---|
| **To Selected Vertex / Edge / Face** | Moves the pivot to the center of the selected components, on the object selected with them or else on their own object. |
| **Reset** | Puts the pivot back on the object's own origin. |
| **Center** | Puts the pivot at the center of the object. |
| **World** | Puts the pivot at the world origin. |
| **Bottom** | Puts the pivot at the bottom center of the object's bounding box. |

### Offset groups

| Button | What it does |
|---|---|
| **Create Offset** | Adds a group named `grp_offset_<object name>` above each selected object, at the same place in the hierarchy, without creating a controller. The object's channels become zero. So `ctrl_arm` gets `grp_offset_ctrl_arm`. |
| **Zero Out** | Moves each selected controller's transform into a new offset group, so its channels become zero. |

### Channels

Check Translate, Rotate, Scale or Visibility, then use the buttons on the selection.

| Button | What it does |
|---|---|
| **Lock + Hide** | Locks the checked channels and hides them from the Channel Box. |
| **Unlock + Show** | Unlocks the checked channels and shows them in the Channel Box. |

### Locators

| Button | What it does |
|---|---|
| **At Center** | Creates one locator at the center of the selected objects and components. |
| **On Each Object** | Creates one locator per selected object, with its position and rotation. |

## Rig tab

### Constraints

Select the driver first, then the targets. Check **Maintain offset** to keep the targets where they are instead of snapping them to the driver.

| Button | What it does |
|---|---|
| **Parent** | Adds a parent constraint from the first selected object to the others. |
| **Point** | Adds a point constraint. |
| **Orient** | Adds an orient constraint. |
| **Scale** | Adds a scale constraint. |

### Joints

| Button | What it does |
|---|---|
| **Orient to World** | Aligns the selected joints with the world axes. Children stay in place. |
| **Aim at Next Joint** | Aims X at the next joint with Y up. End joints align with their parent. |
| **Show Axes / Hide Axes** | Shows or hides the local axes of the selected joints and their child joints. |
| **Rotation order** | Sets the rotation order on the selected objects or joints, keeping their current orientation. |

## License

Copyright (c) 2026 Enzo Freminet. All rights reserved.
