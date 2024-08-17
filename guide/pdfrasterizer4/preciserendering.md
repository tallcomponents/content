# Pixel perfect rendering

Starting with version 4.0, PDFRasterizer.NET renders pixel perfect due to a complete overhaul of our render engine. 

The following compares the rendering of PDFRasterizer.NET 3.0, 4.0 and Adobe PDF Reader.

## Blending modes

PDFRasterizer.NET 4.0 has implemented all blending modes in contrast with version 3.0. It is possible to see differences in rendering on the next examples.

### Luminosity blending mode

In the following example, a light blue colored rectangle is rendered in the background, an image partially overlaps this rectangle and luminosity blending mode is used for rendering image.

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Blending.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Blending.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Blending.png"/></td>
	</tr>
</table>

The following examples show all blending modes. Each PDF has two images that overlap. Image with blue vertical strips with different opacity is rendered in the background. 
Most left strip is fully transparent most right strip is opaque. Rest strips are semitransparent with growing opacity from left to right.
Foreground image is similar to previous one. It contains horizontal brown strips with different opacity growing from top to down.

### Color

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Color.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Color.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Color.png"/></td>
	</tr>
</table>

### Color Burn

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_ColorBurn.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_ColorBurn.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_ColorBurn.png"/></td>
	</tr>
</table>

### Color Dodge

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_ColorDodge.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_ColorDodge.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_ColorDodge.png"/></td>
	</tr>
</table>

### Darken

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Darken.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Darken.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Darken.png"/></td>
	</tr>
</table>

### Difference

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Difference.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Difference.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Difference.png"/></td>
	</tr>
</table>

### Exclusion

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Exclusion.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Exclusion.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Exclusion.png"/></td>
	</tr>
</table>

### Hard Light

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_HardLight.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_HardLight.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_HardLight.png"/></td>
	</tr>
</table>

### Hue

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Hue.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Hue.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Hue.png"/></td>
	</tr>
</table>

### Lighten

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Lighten.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Lighten.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Lighten.png"/></td>
	</tr>
</table>

### Luminosity

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Luminosity.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Luminosity.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Luminosity.png"/></td>
	</tr>
</table>

### Multiply

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Multiply.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Multiply.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Multiply.png"/></td>
	</tr>
</table>

### Normal

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Normal.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Normal.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Normal.png"/></td>
	</tr>
</table>

### Overlay

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Overlay.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Overlay.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Overlay.png"/></td>
	</tr>
</table>

### Saturation

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Saturation.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Saturation.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Saturation.png"/></td>
	</tr>
</table>

### Screen

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_Screen.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_Screen.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_Screen.png"/></td>
	</tr>
</table>

### Soft Light

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/BM_SoftLight.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/BM_SoftLight.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/BM_SoftLight.png"/></td>
	</tr>
</table>

## Image rendering quality

The following examples show differences in image render quality.

### Enlarged small image

This is example shows how small image is rendered on the page when it is enlarged. This PDF has image of the size 2x2 pixels (read, blue, green and white squares).

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Image_enlarge.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Image_enlarge.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Image_enlarge.png"/></td>
	</tr>
</table>

### Image sharpness

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Image_rgb.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Image_rgb.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Image_rgb.png"/></td>
    </tr>
</table>

### 1 bpc rgb image

When the 1 bpc (bits per component) image is renderd too small, Rasterizer4 renders in much better quality (look at small images of the butterfly).

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Image_rgb_1bpc.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Image_rgb_1bpc.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Image_rgb_1bpc.png"/></td>
    </tr>
</table>

### 16 bpc Lab Color space

Correctly decoded image with 16 bpc in Lab color space from JPX stream.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Image_Lab16b_jpx.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Image_Lab16b_jpx.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Image_Lab16b_jpx.png"/></td>
    </tr>
</table>

### Image's soft mask

Decode parameters in the image's soft mask are accepted. It is possible to see soft mask is semitransparent.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Image_skew_smask_decode.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Image_skew_smask_decode.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Image_skew_smask_decode.png"/></td>
    </tr>
</table>

### Image's soft mask matte color blending

Images's soft mask can have defined matte color which has to be used by rendered to blend soft mask before it's using for masking.

<table>
	<tr>
		<td style="border-width:0;text-align:center;">3.0</td>
		<td style="border-width:0;text-align:center;">4.0</td>
		<td style="border-width:0;text-align:center;">Adobe Reader</td>
	</tr>
	<tr>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Image_smask_Matte.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Image_smask_Matte.png"/></td>
		<td style="border-width:0;"><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Image_smask_Matte.png"/></td>
	</tr>
</table>

## Soft mask in the Graphics state

PDFRasterizer.NET 3.0 does not always apply a soft mask from graphics state during
rendering images.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/GS_SMask_alpha_image.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/GS_SMask_alpha_image.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/GS_SMask_alpha_image.png"/></td>
    </tr>
</table>

## Path rendering

### Stroke and fill with semitransparent color or using blending mode

When stroke&fill operator is used for rendering paths and stroke color is semitransparent or other than normal blending mode is used fill color has not be visible through stroke color.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Path_E.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Path_E.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Path_E.png"/></td>
    </tr>
</table>

### Line Dash Pattern

Dash pattern has to be applied properly. It is possible to see differences in rendering dashed lines mostly on green, violet and pink lines.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Path_G.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Path_G.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Path_G.png"/></td>
    </tr>
</table>

## Shadings

All seven shading types are rendered correctly by PDFRasterizer.NET 4.0. Version 3.0 supports only a few of them.

### Shading type 1

This shading is not supported by PDFRasterizer.NET 3.0.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Shading_type1_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Shading_type1_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Shading_type1_path.png"/></td>
    </tr>
</table>

### Shading type 2

Shading is not applied well for stroking color. Fill shading doesn't uses 'Background' attribute (in this example brown color) of shading definition in Rasterizer3.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Shading_type2_path_D.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Shading_type2_path_D.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Shading_type2_path_D.png"/></td>
    </tr>
</table>

### Shading type 3

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Shading_type3_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Shading_type3_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Shading_type3_path.png"/></td>
    </tr>
</table>

### Shading type 4

This shading is not implemented in Rasterizer3.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Shading_type4_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Shading_type4_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Shading_type4_path.png"/></td>
    </tr>
</table>

### Shading type 5

This shading is not implemented in Rasterizer3.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Shading_type5_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Shading_type5_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Shading_type5_path.png"/></td>
    </tr>
</table>

### Shading type 6

This shading is not implemented in Rasterizer3.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Shading_type6_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Shading_type6_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Shading_type6_path.png"/></td>
    </tr>
</table>

### Shading type 7

Shading type 7 is not implemented properly in Ratserizer3.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Shading_type7_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Shading_type7_path.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Shading_type7_path.png"/></td>
    </tr>
</table>

### Shading & Text

Again shading doesn't uses 'Background' attribute (in this example brown color) of shading definition in Rasterizer3. Therefor some letters are not visible.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Shading_type2_text_B.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Shading_type2_text_B.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Shading_type2_text_B.png"/></td>
    </tr>
</table>

### Shading & Image as mask

1 bpc gray image (map of the sky) is used as mask for shading of type 2. Rasterizer3 is not able to apply mask on shadings. Blue rectangle is rendered in the background to demonstrate mask is rendered well.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/Shading_type2_mask.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/Shading_type2_mask.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/Shading_type2_mask.png"/></td>
    </tr>
</table>

## Transparency groups

Rasterizer3 doesn't implement transparency groups (next only TGs is used) in contrast with Rasterizer4. Rasterizer4 implements only isolated non-knockout TGs. It is possible to see differences in the next examples. Brown rectangle is rendered on the pages. Two blue triangles are rendered in the TG which is combined with background page (brown rectangle) by using different blending modes.

### Isolated non-knockout transparency group & Darken blending

Darken blending mode and transparency 0.753 is used for rendering TG.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/TGB-I-NK.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/TGB-I-NK.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/TGB-I-NK.png"/></td>
    </tr>
</table>

### Isolated non-knockout transparency group & Difference blending

Difference blending mode and transparency 0.753 is used for rendering TG.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/TGC-I-NK.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/TGC-I-NK.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/TGC-I-NK.png"/></td>
    </tr>
</table>

### Isolated non-knockout transparency group & Multiply blending

Multiply blending mode and transparency 0.753 is used for rendering TG. In contrast with previous examples TG contains two rectangles with different colors. First one is blue second one is green. Difference blending mode and transparency 0.753 is used for rendering these triangles.

<table>
    <tr>
        <td style="border-width:0;text-align:center;">3.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast3/TGE-I-NK.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">4.0</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/rast4/TGE-I-NK.png"/></td>
    </tr>
    <tr>
        <td style="border-width:0;text-align:center;">Adobe<br/>Reader</td>
        <td><img src="https://raw.githubusercontent.com/tallcomponents/content/master/guide/pdfrasterizer4/media/comp-rast/adobe/TGE-I-NK.png"/></td>
    </tr>
</table>
