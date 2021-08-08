# Master Degree Projects

I learned a lot of useful and interesting stuff during my Undergraduate degree. In the Master Degree in Computer Graphics, Games and Virtual Reality at URJC, I did many projects that made me understand how the GPU works as well as some cool math! Here are some of the things we did.

## Anaglyph 3D and marker tracking

This is definitely one of the projects I found more fun. Remember when we used to wear silly red and blue glasses in the movies and made everything pop out of the screen? Well, those were analyph glasses and it is an old but effective way of achieving stereoscopic vision of a 2D image. On the screen, two different images are being displayed at the same time, but with a red and blue filter respectively. In the glasses, one of the "lenses" blocks the red light and the other blocks the blue light, so each eye gets to see one of the two images. There are many other techniques to do this, but that might be too long to explain here. In the case of live action movies, each shot needs to be taken using two cameras with slighly different position and direction, imitating the eyes of a person. In virtual reality, instead of displaying one image on top of the other, we simply show one image to each eye.

Stereoscopy makes the viewer able to percieve depth, and tell which objects are closer. This is a feature most predatory animals have, becouse they dont need a specially wide field of view, but they (we) do need to percieve distance correctly. If you take a look at most prey animals, their eyes are usually each in a different side of the head, so they lack of this stereoscopy thing, but are able to see more of their surroundings (some birds almost in 360). Pretty cool huh?

This is one of the thing why virtual reality works so well: it uses a binocular system to mimimc depth. Therefore, the scene needs to be rendered twice (one for each eye) with different camera configuration. There are two ways of placing the virtual cameras for stereoscopic vision: toe-in and off-axis. 

- Toe-in: given an inter-ocular distance (IOD) and a convergence point (CP), we simply separete the cameras and make them point towards CP. This might be the most intuitive and easy way of implementing sterescopy, but it is actually wrong! Don't do it or you will make people dizzy.

- Off-axis: The camera projection matrix has to be configurated in a way that both projection planes exactly coincide. This will prevent vertical parallax from happening, which is the main drawback of toe-in.

![Binocular camera models](images/stereoscopy.png)

Using OpenGL, we can render each frame twice using the two cameras we have defined and merge them with different color filters. 

Now comes the second part of the project: marker tracking. To do this, we used a library called Aruco, which detects the relative position and rotation of a fixed set of predefined labeled markers. This actually does most of the hard work (I have no idea how it does it). We can extract the transform matrix of the marker and apply it to the two virtual cameras. This will make the scene move around when the marker is moved. If we stick the marker to our forhead, we now have some kind of head tracking! Might sound a bit shabby but it works faily well and the result is truly worth it. We can even use more than one tracker and bind an object to each one. For example, I added a gun that can be controlled with an extra marker. It can't shoot though... Too bad!

![Result](images/tracking.png)