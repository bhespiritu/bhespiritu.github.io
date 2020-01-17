---
layout: default
thumbnail: "/images/notretopo.png"
categories: [code]
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# The mathematics behind a basic 3D engine

![image](/images/gravity.gif){:class="blog-img"}

When translating things from 2D to 3D, it is sometimes as simple as sticking another component onto whatever data structure you are using. 
The main problem is when you want to visualise it. You could learn to use a library or some other tool for visualisation.

I'm too stubborn to restructure my code around another library, so I learned early on some basic ways to project 3D data onto a 2D viewing plane.

The easiest way to project 3D points onto a plane is to flatten them. Now that seems like a no-brainer, but let's look into the mathematics behind it.

Let's make a plane that will represent the 2D image that we are outputting. We'll call this the View Plane. 
If we wanted to take a 3D point and "flatten" it onto the View Plane, we would just move it along a direction that is perpendicular to the plane 
until it is on the plane. 
