+++
title = "Blender as a Game Editor"
date = "2025-05-13T17:28:33+02:00"
#dateFormat = "2006-01-02" # This value can be configured for per-post date formatting
author = "edo"
authorTwitter = "" #do not include @
cover = ""
tags = ["gamedev", "tools"]
keywords = ["", ""]
description = "A quick and dirty way of getting a 3d editor in an environment that doesn't have one."
showFullContent = false
readingTime = false
hideComments = false
+++

# Using Blender as an editor for your game

Recently, I took part in the Odin game jam.
This was a week-long jam where the only constraint was the us of the odin programming language to write the game.

Odin is a really cool language, especially for no-engine gamedev. 
It has many of the features that jai has + we can actually use it right now!
Despite the cool language features, it is still just a "fancy c" and the most abstract interface you get (for gamedev) is probably using raylib. 
This means you write everything in code, no fancy GUI to hold your hand. 

While I like this workflow, sometimes the editor is really useful.
For example when designing a level for your game or tweaking item positions - this is a nightmare if you have to do it by hand by changing offsets in code.

To top it all off, I was making a 3D game, so placing stuff and making the level would me almost impossible without some kind of visual editor.

## The solution

Blender has a nice 3D editor - Can I somehow use it to make my level, texture it, place all of the items and then export the result into my game?
Well turns out you can, but you have to get a little bit creative.

You can easily export your 3d model as a glft file or something, with all of the meshes packed inside the file.
But I also need to know the positions, sizes and textures of the individual walls (meshes) so I can texture them and compute collision geometry!

To get all the information I needed, I used the "custom properties" field on blender objects.
Turns out the gltf format has a place for you to store whatever [custom data](https://registry.khronos.org/glTF/specs/2.0/glTF-2.0.html#reference-extras) about your mesh!

> When exporting from blender, you have to check the "include custom properties" checkbox for this to work.
> Also, formats other than gltf may not support this.

Now all that remains is to load the data into the game and we are set.
But the raylib `LoadModel` function does not support loading this "extra" data.

Our data is in the glft json section, but since the blender exported models use binary gltf, wf have to do more digging to get it.
Looking at the [format](https://registry.khronos.org/glTF/specs/2.0/glTF-2.0.html#glb-file-format-specification) of the file, we see it consits of a header, then a chunk of json data and lastly a binary chunk containing the model data itself.
Writing a parser for this file and getting the json automatically should be easy enough, but I was lazy so I just openend the file in a text editor and yanked the json manually.

With that, this is the pipeline I used in the end:
1. model everything in blender
2. export to gltf
3. (manually) extract the custom data json to a file
4. load object data from the json file
5. use the object data to create collision geometry and texture the meshes (meshes loaded normally from the exported gltf file)

I also created a similiar process for placing items. 
Here I didn't use the exported mesh, only the item positions and data to draw the corresponding item model based on the item type.

## Conclusion

With this setup in place, I managed to create a pretty large 3d scene very quickly and easily.
I spent quite a bit of time figuring all of this out, so in the end I didn't have time for gameplay code and the game ended up kinda unfinished.
I still had a lot of fun making it though.

While this setup will never be better than having an actual in-game editor, its certainly cheaper to setup than trying to implement all of the blender niceties in an environment without a good 3d editor.
To top it all off, its totally language and framework agnostic!
Additionally, I already have the pipeline figured out - so in future projects, I can use it right away!

If you are interested, you can find the game on [itch](https://ddcveng.itch.io/elbow-grease). The source code is also available on my [github](https://github.com/ddcveng/ElbowGrease/tree/main).
