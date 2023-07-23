# UnitySDF

Signed Distance Field asset importer for Unity; append +sdff to the end of an image file name.

The image should originally be high resolution, and the unity Import Setting "Max Size" should be used to scale it down.

## Generation

This library can automatically convert mask textures to SDFs on import, or can be used manually either in editor or at runtime.

### Non-destructive
Append +sdf to the end of a filename to import the file as a signed distance filed. There are several options.

| Postfix | Effect |
|---------|--------|
| +sdf | Import the image as an SDF file, using the current import settings |
| +sdff | Overwrite the import settings to the recommended Alpha8, and turn off sRGB for 4-channel textures |
| +sdf4 | Perform SDF for each of the 4 channels, not just the alpha channel |

### On-Demand or at Runtime
Create an instance of the SDFGenerator ScriptableObject to hold the configuration options you want, or create one at runtime.

#### In Editor
Fill out the "Targets" list with the textures you want to convert, and press Generate. Each texture will have a duplicate in the same folder with a postfix of ".sdf".
#### At runtime
Call the Generate method on a SDFGenerator instance. The method will return a an SDF Texture2D.

## Usage
An SDF-aware shader is included at Sprites/SDFDisplay.
It supports: 
- Dilation
- Emboss
- Outline
- Underlay (shadow)
- Face Texturing

## Other Info
Internally it uses the Jump Flood Algorithm on GPU to generate the distance fields. The shader "Internal/SDFGenerator" contains the passes required; the first pass calculates the coordinates to the nearest edge and propagates edges outwards, and the second pass calculates distances from those edges.

## Common Problems
**Sprites become very blurry**: Make sure you are using an SDF aware shader.

**Drop shadows are cut off**: The sprite may need to be larger to account for the inflated size once adding a shadow or large outline.

**Image has lost its colour**: Do not use +sdff mode, and ensure the Format is something sensible (and not Alpha8).

**Edges are wonky**: Turn off texture compression; instead reduce the texture size by reducing its resolution, or find a balance of resolution and compression quality.