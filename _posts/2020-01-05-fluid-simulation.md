---
layout: default
thumbnail: "/images/fluid.gif"
categories: [code]
---

# Fluid Simulation

![image](/images/fluid.gif){:class="blog-img"}

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

This is a rework of an old project from high school. I was able to find a chapter of a book by NVIDIA that
explains both the mathematics and the implementation process of making a basic 2D fluid simulation. 
The book, GPU Gems, is an excellent resource for anybody that has an interest in Computer Graphics. 
The book is available [online](https://developer.nvidia.com/gpugems/gpugems/part-vi-beyond-triangles/chapter-38-fast-fluid-dynamics-simulation-gpu) and I highly advise checking it out.


#Bonus

So this simulation is built on the famous Stable Fluids paper by Jos Stam.
As the name implies, the simulation doesn't tend to go haywire. 
However, stable usually leads to more "boring behavior" (Me, 2020).
Because of this, we can make things a little more interesting by modifying the simulation to use the MacCormack scheme.
The MacCormack scheme leads to more visually interesting flow patterns. As you can infer, it is not very stable.
We can code in limiters to keep things stable, but where is the fun in that?
Just for fun, I disabled the limiter. I tried rendering a gif of it, but it's hard to compress it to a reasonable size without losing too much detail. 
Here are some frames instead:

![image](/images/broken.png)

 The video would have been an epilepsy hazard anyway.
It looks pretty cool at first. However, there is a point where something gets so chaotic that it loops back into being boring.

Source Code: [Link](https://github.com/bhespiritu/fluid-simulation)