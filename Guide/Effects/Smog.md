### Turning square fog into round fog


After parsing parameters from UnrealEngine 2 in Unity, we get an incomprehensible result. The texture is too sharp and noisy, the colors are too harsh. Everything points to something else being used in the original.

Original texture
![img1](https://i.ibb.co/hRS9LQyH/oroginal-2.png)    


#### 1. Brightness Normalization and Color Alignment (Luminance)

![img2](https://i.ibb.co/Dgj0t4zJ/shader-1.png)    


Problem: Different frames in the atlas have different colors and densities (pink spheres vs. gray smoke).
Solution: Convert RGB to luminance.
Node: Dot Product.
 > Input A: Texture RGB.  
 > Input B: Vector3(0.21, 0.71, 0.07).  
 > Result: A pure black and white intensity mask,   independent of the original texture color.  

#### 2. Range compression and "cotton" structure (Contrast Logic)

Node: Remap (In: 0..1, Out: 0.15..0.6). 
 > Compresses the dynamic range, removing "black holes" and preventing "overexposure" (the "drip" effect).
Node: Power (0.4 - 0.7). Use a value less than one to "inflate" the midtones. This turns thin strands of smoke into wide, soft flakes 

#### 3. Procedural Rounding (Radial Alpha Mask)

 > Goal: Erasing mesh corners (Quad) and hiding straight texture cuts (sphere halves).   
 > Chain: UV (Static) -> Distance (to 0.5, 0.5) -> One Minus -> Power (1.5) -> Smoothstep.  
 > Smoothstep Parameters:  
 > Edge 1: -0.01 (or 0.2 for cutting blades).  
 > Edge 2: 4.0 (A special hack for normalizing the center brightness, turning a dense sphere into a transparent haze).  
 > Result: A soft clot mask that makes the particle edges completely transparent until they reach the mesh boundaries.  

Result:
![img3](https://i.ibb.co/HLkyS21s/original-3.png)  

Soft cloud almost like the original! 