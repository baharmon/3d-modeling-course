# Contents
1. [**Section elevations**](#section-elevations)
    1. [Boolean operations](#boolean-operations)
    2. [Clipping plane](#clipping-plane)

# Section elevations
This section covers two methods for creating sections or elevations of your 3D models in Rhino.
The first method, Boolean Operations, changes your geometry.
The second method allows you to view a section of your model without changing the geometry.

## Boolean operations
This process will permanently change the geometry of your structure. It is a destructive process.
Before testing this method, make a copy of your 3D model so you have a backup.

### 1. Split

Ideally you will join all your walls/edges of your 3D model.

When you are ready to split your model into your sections, you'll start by drawing
a plane to cut your model with.

Pick the `Rectangle tool, select corner to corner`
Draw a Rectangle bigger than your house either in Front or Right/Left view, depending on
where you would like to take your section.

``_Split
select building > enter
select cutting object (the plane you just drew) > enter
Delete plane.``

You now have your structure, split into two solid sections.
If your walls are split open, use `_Cap` command to close walls into solids.

### 2. Boolean Difference

Draw a cutting plane through your building as above, using Rectangle, corner to corner.
Make sure you draw it in Front or Right/Left view and make it larger than your building or model.

`_boolean difference
select object
select cutting plane``

This process will create a thickness to the cut like a band saw because you used "boolean difference"
to subtract a solid object from your solid model.

## Clipping Plane

This second process for creating section/elevations is not destructive. In fact,
Clipping Planes visually clip your model in a specific viewport. This allows you to see a
section of your building in one viewport while having your model
wholly in tact in another. You can quickly create and manipulate section/elevations
to export for rendering and other uses without permanently changing your model.

First, you will select which windows you wish to 'clip'. Find the Clipping Plane Properties
under the `Visibility tab > Clipping Plane`. Select the viewport you wish to take your
section or elevation view from.

Draw your clipping plane in the appropriate viewport. Make sure it is larger than your building.
You can drag the plane forward and backward, up and down and see your building appear and
disappear behind the plane.
