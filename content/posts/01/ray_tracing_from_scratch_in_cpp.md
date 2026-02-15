---
date: '2026-02-15T23:34:40+03:00'
draft: false
summary: ray tracer
weight: 1
tags: ["Computer Graphics", "Game Development"]
title: 'Building a Ray Tracer from Scratch in C++ Part 1: Mathematical Foundations'
cover:
    image: image.webp
---

Graphics APIs and game engines make rendering look easy, but they hide most of the interesting details. I wanted to understand what actually happens under the hood, so I decided to build a small 3D renderer from scratch using C++.
By the end of this article, you'll understand the core ideas behind ray tracing and be able to build a simple renderer yourself.
We'll finish by rendering the image shown below, without using any graphics libraries or engines but with C++ and Maths.

![Ray Traced Image](/images/image.webp)

### Why RayTracing
In computer graphics, there are two main rendering techniques: **rasterization** and **ray tracing**.

You might ask, what exactly is rendering? In simple terms, rendering is the process of generating a 2D image or video from a 3D model or scene. It's considered the final stage of a 3D engine.

While rasterization is fast and used in real-time applications like games, ray tracing simulates how light behaves in the real world to produce highly realistic images, shadows, and reflections. It traces the path of light rays as they interact with objects in a 3D scene, calculating reflections, refractions, and accurate lighting effects.

Ray tracing involves some basic mathematics. Oops, I just mentioned our worst nightmare 😟 Don't worry though, it's not as scary as it sounds. I've got you. So without any further ado, let's get started.


> One drawback of ray tracing is performance. However, there are optimization techniques we can use to improve speed, which we'll touch on other part.

The below image is generated using ray tracing algorithm

![Ray Traced Image](/images/Glasses_800_edit.png)

### How Ray Tracing Works

Ray tracing simulates how light travels. Rays are cast from a single point in 3D space which is the camera into the scene through an imaginary viewport. Each ray corresponds to a pixel on the screen. When a ray intersects an object (like a sphere or triangle), we compute the closest intersection point to determine the final color of that pixel.

This process is repeated for every pixel, creating a complete, realistic image.

![Viewport](/images/ray_trace_viewport.png)

we have prepared a pipeline to render a simple shape, we will improve it as we go deeper in the subject.

1. Camera → rays
2. Ray → Object intersection
3. Drawing a pixel on the screen

### Let's shoot some rays from the camera

A **ray** is similar to a line but with an important difference a line extends infinitely in both directions, while a ray starts at a specific point and extends infinitely in one direction.

Mathematically, a ray can be represented using a **parametric equation**

![parameteric equation](/images/lagrida_latex_editor%20(13).png)

Where:
- *P* is any point along the ray
- *O* is the ray's origin (starting point)
- *d* is the direction vector
- *t* is a scalar parameter

If *`t = 0`*, then *`P = O`*.

If *`t = 1`*, then *`P = O + d`*.

Increasing *`t`* moves the point along the ray indefinitely.

let's say we have two points in a space "point A" and "point B", let's use 2D for now to better understand the above formula.

A = (0, 0) and B = (4, 4)

> I will use a platform called desmos for plotting points, you can search it online

![GraphAB](/images/Screenshot%202026-01-27%20095929.png)

As we can see in the graph, we have two points, and we want to draw a line segment or a ray between them. A line is defined as an infinite set of points, so how can we represent the line that goes through points A and B? We use the parametric equation of a line. Before writing that equation, however, we need to determine the direction in which the line or ray is pointing. We can find this direction by subtracting point A from point B. The result is a vector, and this vector becomes the direction vector d in the parametric equation

Example:

![latex]()

now we have the direction, if you look closely the vector **d** is the same as the point B, this is when the origin starts from 0, but if it was another point in the space it would be different.

let's plug the direction in the equation and draw our line, but there is one missing piece, the ***"t"*** right? don't worry we don't have to do anything for 't' it has to start from 0.0 and increase it infinitly

![parameter latex](/images/lagrida_latex_editor%20(3).png)