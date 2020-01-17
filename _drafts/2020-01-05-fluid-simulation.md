---
layout: default
thumbnail: "/images/notretopo.png"
categories: [code]
---

# Gravity Simulation

![image](/images/gravity.gif){:class="blog-img"}

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

NOTE: The GIF has a lot of artifacts from image compression. I decided to keep it that way because it looked cool.

A while ago, I stumbled upon [a blog post by Ricky Reusser](https://observablehq.com/@rreusser/2d-n-body-gravity-with-poissons-equation) 
that demonstrated how one could simulate gravity on the GPU using something called a Poisson Equation.
The resulting simulation is absolutely mesmerizing. I instantly wanted to recreate it for myself.

### Underlying Mathematics

In the blog, he goes into detail about the underlying mathematics as well as different methods of integrating the simulation.
The method he uses utilizes Fast Fourier Transformations in a way that I still don't fully comprehend (I swear, FFTs are literal black magic.)
For my simulation, I used the much-easier-to-implement Jacobi method.

Now that was a lot to take in. Let's break down what exactly is happening in the image.
What we are seeing are a bunch of identical little particles experiencing gravitational forces as described by Newton's law of universal gravitation.
Just using that one force, we get these spiraling galaxy-like formations. That is incredibly elegant if you ask me.

There are two major simplifications that will make things easier to implement:
1. Each particle is identical in mass and so small that they will never collide with each other.
2. The entire world loops around on itself. If you want to get technical, we would be mapping the world onto a torus.

Now the naiive approach to implementating something like this is to take every possible pairing of particles and calculate the gravitational forces between them.
The runtime grows exponentially with the number of total particles. I want for this to be able to run in realtime. To optimize, we sort of clump together particles that are in close proximity. 
This is done by creating a grid across the whole world. We then store the number of particles within each gridspace. 
Since each particle is identical, we can multiply the number of particles by their mass to get a mass distribution. 
From the mass, we can calculate the density by choosing an arbitrary "area" for each gridspace. 
For the sake of simplicity, we can just say the area of each gridspace is 1 square meter.

Now is where things get interesting. Let's introduce the Poisson equation.
\begin{align}
\nabla^2 \phi = 4 \pi Gp
\end{align}

In the above equation $$\phi$$ represents gravitational potential, G is the Gravitational constant and p represents density. 
Using this equation, we will be able to calculate gravitational potential field of the world. 
However, there is one problem: What the hell is $$\nabla^2$$?

$$\nabla^2$$ is an operator known as a Laplacian Operator. 
To put it in technical terms, the Laplacian operator is the divergence of the gradient of a field. (Hence, $$\nabla * \nabla = \nabla^2$$).
I don't fully understand the nuances of it myself, so I don't think it's my place to attempt to explain it.
All you need to know is that when you have an equation of form: $$Ax = b$$ where $$A=\nabla^2$$, there are various methods we can use to apply to solve for x.

The simplest way of doing this is the Jacobi method. This is where we take some random number and repeatedly apply an equation to adjust it's value until it converges to x.

In the context of a grid and the base form $$\nabla^2x = b$$, the equation looks like this:

\begin{align}
x_{i,j} &= \frac{b_{i-1,j} + b_{i+1,j} + b_{i,j-1} + b_{i,j+1} - c*x_{i,j}}{d}
\end{align}

i and j correspond to the x and y coordinate within the grid. c and d are some predefined constants that depend on the equation we are working with.

For our problem, $$x_{i,j}$$ represents the potential field we want to calculate. $$b_{i,j}  = p$$  where p is the density field we calculated before. $$c = 4 \pi Gp$$ which is the coefficient from the Poisson equation.
Finally, d is simply equal to 4. 

I would iterate through the entire graph 20 times. 
Since I care more about visual appeal than physical accuracy, this number was chosen arbitrarily and adjusting it can lead to interesting changes in the final product.
Technically, the more you iterate, the more physically accurate your simulation becomes.

### Visualization

To render the gif at the beginning of the article, I made use of Java's builtin Swing library. It's outdated and difficult to learn to use well,
but I find that it lets me throw together a prototype without going through the hassle of downloading an extra library.

To be Continued.

Source Code: [Link](https://github.com/bhespiritu/physics-toys)