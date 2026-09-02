# Maya / AE Overscan Camera Calculator

A lightweight browser-based tool for calculating an overscan camera in Autodesk Maya when working with resized or repositioned footage in After Effects.

It keeps the original camera match intact while calculating the required:

- Render resolution
- Camera Aperture
- Film Offset
- Maya camera setup script

No installation or external dependencies are required.

---

## Usage

Open `index.html` in any modern web browser.

### 1. Enter the original footage size

Under **Footage / Plate**, enter the resolution of the footage that the original Maya camera was matched to.

Example:

```text
1518 × 800
```

---

### 2. Enter the target composition size

Under **Target Comp / Render**, enter the resolution of the After Effects composition you want Maya to render.

Example:

```text
1920 × 1080
```

The target composition size will also be used as the Maya render resolution.

---

### 3. Enter the original Maya Camera Aperture

In Maya, open the camera attributes:

```text
cameraShape
→ Film Back
→ Camera Aperture
```

Enter the **Horizontal** and **Vertical** aperture values into the calculator.

The tool supports both:

- inches
- millimeters

Also select the same **Fit Resolution Gate** mode used by the Maya camera:

```text
Horizontal
```

or

```text
Vertical
```

This setting is important because it determines which film aperture axis controls the camera projection.

---

### 4. Enter the After Effects layer transform

If the footage is centered and unscaled, use its normal AE transform values.

Otherwise enter the actual:

- Anchor X
- Anchor Y
- Position X
- Position Y
- Scale X
- Scale Y

You can use **Anchor = Footage Center** to quickly set the anchor point to the center of the source footage.

> Rotation is currently expected to be `0°`.
>
> If the footage is rotated in After Effects, pre-compose it first or remove the rotation before calculating the overscan.

---

## Maya Results

The calculator outputs the values required for the overscan camera.

### Render Settings

Set Maya's render resolution to the displayed target size.

```text
Render Settings
→ Image Size
```

### Camera Aperture

Copy the calculated values to:

```text
cameraShape
→ Film Back
→ Camera Aperture
```

### Film Offset

Copy the calculated values to:

```text
cameraShape
→ Film Back
→ Film Offset
```

If the offset moves in the opposite direction in your Maya setup, invert the sign of the Film Offset values.

For example:

```text
0.053  0.079
```

becomes:

```text
-0.053  -0.079
```

---

## Maya Script

The calculator also generates a Python script containing the calculated values.

To use it:

1. Duplicate the original camera.
2. Select the new overscan camera in Maya.
3. Copy the generated Python script.
4. Open Maya's Script Editor.
5. Switch to a Python tab.
6. Paste and run the script.

The script automatically sets:

```text
horizontalFilmAperture
verticalFilmAperture
horizontalFilmOffset
verticalFilmOffset
render width
render height
```

Camera Aperture and Film Offset are written in **inches**, which is how Maya stores these attributes internally.

---

## Important

Do **not** change:

```text
Focal Length
Translate
Rotate
```

The purpose of the overscan camera is to preserve the original camera match.

Only the following values should normally change:

```text
Camera Aperture
Film Offset
Render Resolution
```

---

## Fit Resolution Gate

The calculator respects Maya's **Fit Resolution Gate** behavior.

When using **Horizontal** fit, the horizontal aperture controls the projection. The required vertical aperture is derived from the target render aspect ratio.

When using **Vertical** fit, the vertical aperture controls the projection and the horizontal aperture is derived from the target render aspect ratio.

This is important when the original camera's film aspect ratio does not match the source footage aspect ratio.

---

## Visual Check

The preview shows:

- **Outer frame** — target composition
- **Inner area** — original footage
- **Center marker** — footage center

This provides a quick way to verify the expected placement before applying the values in Maya.

The calculator also displays:

- footage bounds
- distance from the composition center
- aperture multipliers
- approximate Blender Camera Shift values

---

## Example

Original footage:

```text
1518 × 800
```

Target composition:

```text
1920 × 1080
```

Original Maya Camera Aperture:

```text
Horizontal: 1.417
Vertical:   0.945
```

Fit Resolution Gate:

```text
Horizontal
```

Example calculated aperture:

```text
Horizontal: 1.792
Vertical:   ~1.008
```

The camera can now render the larger composition while preserving the framing of the original footage.

---

## Requirements

None.

The calculator is a standalone HTML/CSS/JavaScript application and runs locally in a browser.

No server, build process, npm packages, or internet connection are required.

---

## License

GPLv3
