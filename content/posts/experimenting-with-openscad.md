+++
slug = 'experimenting-with-openscad'
title = 'Experimenting with OpenSCAD'
date = 2025-02-10T00:00:00+00:00
draft = false
tags = ["Ham Radio", "3D Printing"]
+++

So far I have made all my 3D designs with Tinkercad. Although this browser-based CAD program is very limited in its range of functions, it can still be used for many things - such as [my latest project](/spiderbeam-hd-12m-mount/). I recently found out about [OpenSCAD](https://openscad.org/) and learned that you don't drag objects onto the workspace with the mouse, change their size, move them, rotate them, etc., but that you create the 3D object programmatically. This means that you write the code in a text editor that defines the 3D object and you can then render a 3D preview. The object can then be exported as an STL, for example, so that it can then be sliced and finally printed.

The project linked above was well suited for initial experiments, so I have now implemented it in OpenSCAD. The idea was to parameterize the object in such a way that you only have to adjust a few variables to be able to print it for other pipe diameters, for example. So far I have only succeeded to a certain extent. 

With the following code...

```openscad
spiderbeam_diameter = 55;   // diameter if the inner hole holding the Spiderbram mast
clearance = 0.6;            // additional clearance added to the spiderbeams's diamater
metal_post_diameter = 42;   // diameter of open metal post part, fixated with hose clamps
object_height = 100;        // height of the resulting object
hoseclamp_width = 10;       // with of the hose clamps, defining the length of the slots
$fn=100;

difference() {
    union() {
        cylinder(object_height, d = spiderbeam_diameter + (spiderbeam_diameter*0.5));  // define the outer cylinder

        // subtracting the hole from the cube
        difference() {
            translate([-(metal_post_diameter-5)/2, -(spiderbeam_diameter+10), 0])  // aligning the cube with the cylinder
            cube([metal_post_diameter-5, metal_post_diameter, object_height]);  // set cube dimensions
        }
    }
    
    // Main hole through the cylinder (for the Spiderbeam mast)
    cylinder(object_height, d = spiderbeam_diameter + clearance);  // Hole for the Spiderbeam mast lower tube with some clearance
    // adds the two holes for both hose clamps
    for (hose_clamp_z_pos=[((object_height/100)*20),((object_height/100)*80)-hoseclamp_width]) {
        difference() {
            clamp_position = [0, -(spiderbeam_diameter+30/2-5), hose_clamp_z_pos];
            translate(clamp_position) cylinder(hoseclamp_width, 30, 30);
            translate(clamp_position ) cylinder(hoseclamp_width, 27.5, 27.5);
        }
    }   
    // Hole inside the cube (for the metal post)
    translate([0, - spiderbeam_diameter - 10, 0])  // Center the hole in the cube
    cylinder(object_height, d = metal_post_diameter);
}
```

...this object is then generated:

![](/img/experimenting-with-openscad-01.jpg)

I like the reproducibility and the simple programmatic possibility to customize/modify the 3D object, but it is much more time-consuming to create such an object in OpenSCAD than in Tinkercad, for example. 
