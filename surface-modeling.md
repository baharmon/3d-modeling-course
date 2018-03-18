# Contents
1. [**Surface modeling**](#surface-modeling)
    1. [Manipulating Surfaces](#Manipulating-Solid-Surfaces)


# Surface modeling
You can create forms and solids in several ways in Rhino, through points, lines, meshes and solid shapes.
There are standard shapes in your toolbar in Rhino that you can use to quickly build
your form and then manipulate the surfaces directly. This section provides some tips for quickly manipulating
these solids.

## Manipulating Solid Surfaces

We will manipulate a solid through subobject selection and manipulation.

Using the `_Box` command, draw a solid cube.

You will extract a single surface from this solid to manipulate, then join the surfaces back together
before you change the shape so that it remains solid.

``_ExtractSrf > _Split > select isocurve > _join``

You should now have a solid but be able to select an individual surface to manipulate.

`Control-shift-click` to select the surface you just extracted.
`Click (on the gumball) > Control > Drag` to manipulate the surface with the gumball tool.

 
