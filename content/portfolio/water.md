+++
title = "Water Shading"
portfolioCover = "water-shader.png"
#tags = ["c++", "opengl", "graphics"]
+++

This project is a demo scene showcasing a water shader I made as a school project.
The water reflects the scene above it and refracts the scene below. 
The waves are achieved by sampling normals from a normal map texture to calculate the lighting.
It looks really nice when the texture is animated and the waves move over the surface of the water.
When moving around the scene, you can see the frenel effect on the water (ratio of reflected vs refracted light). 

There is also some depth-based fog in the deeper parts of the pool, but it doesn't work very well and depends on the depth from the camera as opposed to the depth from the water surface.

The source code can be found on my [github](https://github.com/ddcveng/VolumetricWater). 
My setup is visual studio specific but it should work fine on linux as well.
I implemented it using `C++` and `OpenGL`.

{{< image src="/water.png" alt="water reflection" position=center >}}
{{< image src="/water2.png" alt="water refraction" position=center >}}
