# Particle System Porting (Original Game Breakdown)

After 8 months of reverse-engineering the particle system in the original game and several iterations that improved porting accuracy, I have finally pieced together the big picture.

## 🛠 Overall Porting Process

1. **Creating the Fitting System:** Develop a system and formulas to fit and adapt the effect `.uc` files from the original game into the target engine.
2. **Color Scale Logic:** Write a system that replicates the `Color Scale` logic (this is critical).
   * The `ColorScale` system calculates color based on incoming data: `Config Color 1`, `Config Color 2`, `Particle Lifetime`, and Particle `Alpha channel`.
   * This produces beautiful gradients/halftones in the game itself, mostly affecting the `SpriteEmitter`.
3. **FadeOut / FadeIn:** Implement a system to replicate `FadeOut`/`FadeIn`. It plays a vital role in the visual setup: it doesn't just make the particle transparent, it also subtly alters its color.
4. **Blending Modes:** Find the correct system for blending particles with the background, specifically the `Blending (Additive)` mode. This ensures the correct look for effects featuring particles with soft edges/halftones or inner plasma cores.
5. **Shader Decompilation:** Locate shader decompiles. This is highly important for fog effects, as that is where the blending occurs.
6. **Geometry Formulas:** Figure out the formulas for geometry. Frankly, there aren't that many—roughly 10–20 core formulas and libraries.

> **Result:** Ultimately, you will get a pixel-perfect, byte-accurate replica of the effect.

---

## 🔍 Detailed Breakdown & Current Progress

### What is implemented so far?
I have built a system that logs every single `Tick` of the original game's particle system. The data is logged in the following format:
* `oldPosition`
* `currPosition` *(currentPosition)*
* `Size`
* `Scale`
* `Opacity`
* `Color[]`
* `Speed`
* `Drag`
* `Acceleration`

Using these logs, I have derived about **8 production-ready formulas** for how geometry behaves in the game.

💡 **The most important part:** Now I can validate the formulas I discover. I take data from `Tick 1`, use the formula to calculate the state for `Tick 2`, and then cross-check the result against the actual data logged from `Tick 2`.

### 📈 Fresh Data on Emitters

Matching a `SpriteEmitter` from a `.uc` config to my game world scale has always been a major headache. Thanks to the logger, I managed to figure out the exact porting formulas.

#### 1. Sprite Porting
```math
localScale = StartSizeRange × 52.5(UE) × SizeScale(t) × K_world
```
> Where `K_world` is your world scale coefficient. For instance, in the original game, the player height is `1.8m`, whereas in my engine (Unity), it is `0.85m`. This discrepancy must be compensated for.

#### 2. Mesh Porting
```math
localScale = StartSize × SizeScale(t) × K_world
```

### 📋 Conclusion
The `StartSize` parameter is calculated differently depending on the emitter type (`Sprite` or `Mesh`). 

I tested these calculations on sample effects, and the data matched **byte-for-byte**. Using this logger allows for reverse-engineering any complex formula or blending mode, taking asset porting to a whole new level.
