---
{"dg-publish":true,"permalink":"/image-processing/before-mid/intensity-transformation-and-spatial-filtering-summary/"}
---

### Part 1: Intensity Transformations (Point Processing)

These techniques operate on a single pixel ($1 \times 1$ neighborhood) at a time, mapping an input intensity $r$ to an output intensity $s$.

| **Technique / Filter**  | **Category**     | **Mathematical Concept**                                        | **Primary Purpose / Application**                                                                                                                            |
| ----------------------- | ---------------- | --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Image Negative**      | Linear           | $s = L - 1 - r$                                                 | Reverses intensity levels. Used to enhance white or gray details embedded in large dark/black regions <br><br>(e.g., medical X-rays).                        |
| **Log Transformation**  | Logarithmic      | $s = c \times \log(1 + r)$                                      | Expands a narrow range of dark input values while compressing higher-level values. <br><br>Used to reveal hidden details, like in a Fourier spectrum.        |
| **Power-Law (Gamma)**   | Exponential      | $s = c \times r^\gamma$                                         | Lightens ($\gamma < 1$) or darkens ($\gamma > 1$) an image based on an exponent. <br><br>Used for **Gamma Correction** to fix nonlinear monitor displays.    |
| **Contrast Stretching** | Piecewise-Linear | $I_{new} = (I - Min)\frac{NewMax - NewMin}{Max - Min} + NewMin$ | Stretches a narrow intensity range to span a wider target range (usually 0 to 255). <br><br>Used to normalize images with poor illumination.                 |
| **Thresholding**        | Piecewise-Linear | $s = 1$ if $r \ge m$, else $s = 0$                              | Converts grayscale to binary (black and white) based on a threshold $m$. <br><br>Used for **image segmentation** (e.g., separating objects from background). |

---

### Part 2: Smoothing Spatial Filters (Neighborhood Processing)

These filters use a moving mask (e.g., $3 \times 3$) to calculate a new value for the center pixel based on its surrounding neighbors. Their main goal is noise reduction and blurring.

| **Technique / Filter**     | **Category**                | **Concept / Mask**                                                                                                     | **Primary Purpose / Application**                                                                                     |
| -------------------------- | --------------------------- | ---------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Box Filter (Averaging)** | Linear Spatial              | All mask coefficients are equal (e.g., all $1$s, divided by $9$). Replaces center pixel with the neighborhood average. | Blurring an image and reducing random noise.<br><br>_Drawback:_ Causes significant blurring of edges.                 |
| **Weighted Average**       | Linear Spatial              | Center mask coefficients have higher values than outer ones.                                                           | Smoothes the image while preserving slightly more detail/edges compared to a standard box filter.                     |
| **Median Filter**          | Order-Statistic (Nonlinear) | Replaces the center pixel with the **median** value of the neighborhood.                                               | Excellent for removing impulse noise (**salt-and-pepper noise**) with considerably less blurring than linear filters. |
| **Max Filter**             | Order-Statistic (Nonlinear) | Replaces the center pixel with the **maximum** value of the neighborhood.                                              | Finds the brightest points in an image; effectively removes "pepper" (dark) noise.                                    |
| **Min Filter**             | Order-Statistic (Nonlinear) | Replaces the center pixel with the **minimum** value of the neighborhood.                                              | Finds the darkest points in an image; effectively removes "salt" (bright) noise.                                      |

---

### Part 3: Sharpening Spatial Filters (Neighborhood Processing)

These filters use spatial differentiation to highlight rapid transitions in intensity, effectively sharpening edges and fine details.

|**Technique / Filter**|**Category**|**Concept / Math**|**Primary Purpose / Application**|
|---|---|---|---|
|**Laplacian Filter**|Linear Spatial (2nd Derivative)|Isotropic mask (e.g., center $-4$ or $-8$, surrounded by $1$s). $\nabla^2 f = \frac{\partial^2 f}{\partial x^2} + \frac{\partial^2 f}{\partial y^2}$|Highlights fine details and rapid intensity transitions. The result is usually added back to the original image to restore background tones.|
|**Unsharp Masking**|Image Arithmetic|$g(x,y) = f(x,y) + [f(x,y) - \bar{f}(x,y)]$|Subtracts a blurred version of the image from the original to create an edge mask, which is then added back to sharpen the image.|
|**Highboost Filtering**|Image Arithmetic|$g(x,y) = f(x,y) + k \times [f(x,y) - \bar{f}(x,y)]$|Similar to Unsharp Masking, but multiplies the edge mask by a weight $k > 1$ to drastically increase the contribution of the edges.|
|**Roberts Cross-Gradient**|Nonlinear Spatial (1st Derivative)|Uses $2 \times 2$ masks to compute diagonal differences.|Extracts edges by calculating the gradient magnitude. Very sensitive to noise due to its small mask size.|
|**Sobel Operator**|Nonlinear Spatial (1st Derivative)|Uses $3 \times 3$ masks to compute horizontal ($G_x$) and vertical ($G_y$) changes.|Extracts edges. Provides a slight smoothing effect due to the $3 \times 3$ size, making it less sensitive to noise than Roberts.|
