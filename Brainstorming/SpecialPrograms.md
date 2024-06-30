# Special Programs
Some programs at first don't seem to fit the POODL model.

## 2D Layout
2D Layout is a POODL program best used with a 2D Shell for laying out printed designs, web pages, and videos. 2D Layout makes use of 3 axis: X, Y, and Time. Printed designs have no Time axis. 2D Layout makes use of Regions. Regions are rectangular areas in 2D space. The have a position (X, Y) and a size (width, height). Each of these are a function of Time. Regions can contain shapes, bitmaps, and other regions.

## 3D Layout
The 3D Layout program is the same as the 2D Layout but includes a Z axis. It's easiest to use the 3D Shell for running the 3D Layout program. You can connect a View Port object to the 3D Layout block and it will output a 2D Layout block. Likewise, you can connect any 2D Layout block to a face of any 3D object in a 3D layout.