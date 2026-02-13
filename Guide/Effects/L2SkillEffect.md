## ⚔️ Notes on Shader Parameters

Use the following parameter configurations. This setup prevents the common "vortex collapsing" issue and ensures smooth energy dissolution along the blade.

### Core Shader Parameters

| Parameter | Technical Description | Lineage 2 Style Configuration |
| :--- | :--- | :--- |
| **StartVelocityRangeX** | **Radial Velocity.** Determines particle movement towards or away from the object's center. | **Set to 0.** Negative values create a "black hole" effect, pulling all energy into a single point at the sword's core. |
| **SizeScale (0-9)** | **Size over Lifetime.** Defines the physical scaling of the mesh/particles over their lifespan. | **Y = 1.0 (constant).** To prevent the effect from shrinking, keep all Y values consistent. Dissipation should happen via Alpha, not Scale. |
| **SpinPerSecondRangeX** | **Radial Spin Dynamics.** Controls how quickly the vortex width changes per second. | **Set to 0.** In the original game, the rotation radius is constant. Any value here will cause the spiral to narrow or expand pulsing. |
| **SpinPerSecondRangeZ** | **Orbital Velocity.** The speed at which the effect rotates around the weapon's axis. | **Recommended: 0.5 – 1.5.** High values create the dense "energy winding" effect characteristic of high-grade soulshots. |
| **StartVelocityRangeZ** | **Linear Velocity.** The speed at which the effect travels from the hilt to the tip. | **Linked to Duration.** Should be tuned with Lifetime. Use low values (e.g., `-0.2`) for a smooth, "magical" crawl along the blade. |
| **FadeoutStartTime** | **Dissipation Timing.** The point in time (0.0–1.0) when the effect begins to become transparent. | **0.6 – 0.7.** In code: `totalLife * 0.7f`. This allocates 30% of the duration for soft fading, preventing "hard" flickering. |



1. **Stable Volume:** Energy maintains its width throughout its lifespan by nullifying **Velocity X** and **Spin X**.
2. **Soft Dissipation:** Instead of collapsing into a point (**SizeScale**), the effect gradually loses brightness and transparency (**Fadeout**).
3. **Dense Spiral:** Visual complexity is achieved by high orbital

### Examples
1. If **StartVelocityRangeX** is set to 0, the particles with the shader will not move to the specified point and will remain in place.
2. If **FadeoutStartTime** m.SetFloat("_FadeoutStartTime", totalLife * 0.7f); attenuation will occur with 70%
3. If **SizeScale (0-9)** SizeScale0 (x:0.03 , y: 1),SizeScale1 (x:0.07 , y: 1.8),SizeScale2 (x:0.08 , y: 1),SizeScale3 (x:0.56, y: 1)
Based on these data, the particle will behave as a sharp pulsation or a "micro-burst" at the very beginning of its appearance, after which it will maintain a stable size.

4. To slow down the particles and keep them within the same radius, decrease **StartVelocityRangeX** and increase **Duration**. This will produce the same radius but slower and smoother movement.


####  Thanks to https://github.com/shnok for creating the base L2SkillEffect shader