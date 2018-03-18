# Contents
1. [**Solid modeling**](#solid-modeling)
    1. [**3D Drafting**](#3d-drafting)
    2. [**Extrusion**](#extrusion)
    3. [**Various 'How To's' for Frank Lloyd Wright Project Building**](#how-tos)

# Solid modeling
*Under development...*

## 3D drafting
*Under development...*

## Extrusion
*Under development...*

## How To's

### Drawing walls.

Use the `_Polyline` tool to draw the outline of your wall in plan view.

Make sure you have `Ortho mode` on and possibly `SnaptoGrid` so your measurements are accurate.

As you draw your polyline you can type in any known wall widths and lengths to keep your frame as accurate as possible.

Make your wall solid.
```
`Solid > Extrude Planar Curve > Straight`  or `_ExtrudeCrv` command.
 Select the curves you want to extrude > Enter.
 Ensure you extrude your solid in the correct direction by using your
 Front or Right view windows.
 Here use you Front window and ensure your wall is
 going up above the ground plane.
 Type in your height of the wall > Enter.
```

Your command line for this process may look like this:
```
Command: _ExtrudeCrv
Select curves to Extrude:
Select curves to Extrude. Press Enter when done:
Extrusion distance <9> ( Direction  BothSides=No  Solid=Yes  DeleteInput=No  ToBoundary  SplitAtTangents=No  SetBasePoint ):
Creating meshes... Press Esc to cancel
1 extrusion added to selection.
```

You should have a solid wall.

If you don't, there are a few things to check.

When you are running the command `_ExtrudeCrv` you have several options that pop up in your command line to review, as seen above. The options you may change within this command are the following:  

`(Direction  BothSides=No  Solid=Yes  DeleteInput=No  ToBoundary  SplitAtTangents=No  SetBasePoint ):`.

If your wall is hollow (has no top) then you likely ran the command with `Solid=No` option checked. Rerun the command and ensure Solid is checked to `Yes`.


### Creating Doors and Windows

This section will review how to create windows + doors using two commands:
 `_BooleansDifference` and `_MakeHole`.

__BooleanDifference__

Start with a solid wall.

Draw a door in plan view where it would intersect with your wall using `_Polyline`

Make your door solid.

```
Either navigate to `Solid > Extrude Planar Curve > Straight`
or type `_ExtrudeCrv` into the command line.
In Front or Right view set the direction you want to extrude your door,
in this case, UP.
Type in your Door height > Enter.
```

Next, we will create your door by subtracting the Door solid from your Wall solid.

Type `_BooleanDifference` into your command line.

First, select your wall > Enter. Next select your door > enter. Ensure that you keep the solid that is your door by checking your command line says `_DeleteInput=No` > Enter.

Your command line for this process may look like this:
```
Command: _BooleanDifference
Select surfaces or polysurfaces to subtract from:
Select surfaces or polysurfaces to subtract from. Press Enter to continue:
Select surfaces or polysurfaces to subtract with ( DeleteInput=No ):
Select surfaces or polysurfaces to subtract with. Press Enter when done ( DeleteInput=No ):
Boolean difference in progress... Press Esc to cancel
Creating meshes... Press Esc to cancel
```
Create a new layer for your door in your Layers Tab on the right side of your screen.

Select your door.

Right click on your new `Door` layer in your `Layers Tab` and select `Change Object to Layer`.

Now you have your door and it is on its own layer so you can manipulate the wall and door materials separately.
