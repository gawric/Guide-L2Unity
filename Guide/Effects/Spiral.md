### An attempt to recreate the spiral effect for SpriteEmitter_4_43

I will be conducting all tests in Blender v3.7

Let's get started.


So, let's start with a mashup model for our spin effect. I found it online as a propeller.
![img1](https://i.ibb.co/MxsfJ4x5/img1.png)    
Judging by its blades, the mash spins counterclockwise.

1. We'll switch to animation mode and select our mash model. We'll see that the pivot isn't exactly in the middle. It's like an anchor around which our mash will rotate.

2. Let's take our mash and move it 5 meters away from the pivot.

3. After this, we will create 1 frame in the animation, create the next frame at number 60 and move the mash 20 meters forward along the z-axis and rotate it along the z- and x-axis as well.

4. When we run the animation, we'll see a rotation and twisting effect. The pivot will move in a straight line, and the mash will rotate diagonally around it because we've tilted the two axes. We've rotated the mash itself counterclockwise by 720 degrees and moved it forward by 30 degrees.

And here is our final version.   

![img2](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExYmhib2JtejB0ZjB2bWM5aG4zNzh0cHprY2g3bXBnMHZhYWM2bG5hbyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/aAptk43YtuB4TH9XWT/giphy.gif)