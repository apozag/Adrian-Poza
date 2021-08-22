# Post-Process Effects in OpenGL

Post-process effects are those which are added on top of the final rendered image. They can be used to increase the feeling of realism in some cases, or simply to add a visual extra something something. You might have spotted a bunch of this effects while playing your favourite videogames. The most popular ones are motion blur, depth of field, bloom, edge detection... But the sky is the limit when ir comes to processing an image.

First of all, lets overview the hole process. First, the scene will be rendered but not displayed. Instead, we store this rendered image in a series of textures, which we will apply a kernel operation on in the next rendering pass. The geometry in the next step will only be a plane that covers the entire screen, and we will map the proccesed image on it. If you are familiar with OpenGL, you will know that the result of the graphic pipeline is a collection of images stored in textures bound to a framebuffer. If nothing is specified, the active framebuffer will be the default framebuffer, this means the result will be displayed on the screen. If we want to do stuff with the image before we show it, we will have to create a new framebuffer pretty much the same way you create any other OpenGL object. Then we have to create and configure the textures that will be bound to our framebuffer. The amount and type of textures will depend on what you want to achieve. In our case, we will only need one texture that stores color values and another to store the depth values. We can actually chain more than one of this post-process operations to add more layers to the effect. For this, we will need at least an extra framebuffer. Here are the results of using different effects: 

Two Gaussian blur kernels on top of each other.
![Blur](https://apozag.github.io/Adrian-Poza/images/blur.PNG)

Edge detection with and without blur on top.
![Edge](https://apozag.github.io/Adrian-Poza/images/edge.PNG)
![Edge](https://apozag.github.io/Adrian-Poza/images/blur-edge.PNG)

Image shapening.
![Sharp](https://apozag.github.io/Adrian-Poza/images/sharp.PNG)