+++
title = "Volumetric terrain rendering"
portfolioCover = "ravine-discrete.png"
+++

I made this project as part of my bachelor's thesis on rendering volumetric data as implicit surfaces.
The main paper I referenced was concerned with rendering 3D terrain. 
My supervisor suggested using the voxel terrain of Minecraft as the input for the rendering method, which I thoughs was hilarious so we went with it. How many people can say they got their degree by playing minecraft?

Basically, I load a minecraft world to get a 3D voxel grid of data. I then run a convolution with a blur kernel over the grid to extract a signed distance function representing the smoothed out terrain. Finally, I use the marching cubes algorithm to polygonize the SDF and render the resulting mesh using the normal pipeline. Yes, mapping and blending the textures onto the implicit surface took some time. 
Oh and its written in Rust, btw.

If you care, the text of the thesis is available [here](https://dspace.cuni.cz/handle/20.500.11956/184418) and the source code can be found on my [github](https://github.com/ddcveng/dd-terrain).

![discrete](/ravine-discrete.png)
![implicit](/ravine-implicit.png)
