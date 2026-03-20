---
{"dg-publish":true,"permalink":"/image-processing/before-mid/lecture-6-filtering-in-the-frequency-domain-ch4/"}
---


- **What it is**: Unlike spatial filtering which alters pixel values directly, this technique processes images by manipulating their frequency components. It uses the Fourier Transform to convert an image from the spatial domain to the frequency domain.
    <br>
- **Why we use it**: It is highly efficient for specific operations like noise removal, sharpening, and blurring. It allows us to target specific image contents: smooth areas correspond to low frequencies, while edges and details correspond to high frequencies.
    <br>
- **Spatial vs. Frequency**:
    
    - **Spatial Domain**: Works with pixel intensities directly (e.g., applying convolution masks like Gaussian blur). It is intuitive but can be computationally heavy for large filters.
        <br>
    - **Frequency Domain**: Represents the image as combinations of sine and cosine waves. Filtering here is done via simple multiplication instead of convolution, making it significantly faster for large filter kernels.
        <br>
- **Practical Example**: The slides demonstrate periodic noise removal using an image of the moon from NASA. The original image suffers from sinusoidal interference. By viewing the magnitude of the Fourier transform, the interference appears as distinct "bursts of energy". A mask is applied to eliminate these bursts, and the inverse Fourier transform reconstructs a clean, filtered image.
    

---

### **The Big Idea & Historical Context (Slides 6-11)**

- **History**: Jean Baptiste Joseph Fourier (born 1768 in France) introduced these concepts in his 1822 work, "The Analytic Theory of Heat". Though largely ignored at publication, it became a foundational mathematical theory for modern engineering.
    <br>
- **Fourier Series & Transform**:
    
    - **Series**: Any periodic function can be expressed as a sum of sines and cosines of different frequencies.
        <br>
    - **Transform**: Even non-periodic functions (with finite area) can be expressed as the integral of sines or cosines multiplied by a weighting function.
        <br>
- **The Prism Analogy**: Just as a glass prism separates light into its component colors (frequencies), the Fourier Transform acts as a "mathematical prism" that separates spatial data $f(x,y)$ into frequency data $F(u,v)$. * **Properties**: Low frequencies cluster near the center (representing overall structure), while high frequencies spread to the outer edges (representing fine details and noise). Importantly, the image can be completely recovered without information loss via the Inverse Fourier Transform.
    

---

### **The Mathematics of Fourier Transform (Slides 12-28)**

The lecture breaks down the transform into four distinct cases:

**1. 1-D Continuous & Discrete Signals (Slides 13-20)**

- **Continuous**: Used for continuous signals like analog audio. The transform and its inverse rely on integration.
    <br>
- **Discrete (DFT)**: Used for sampled signals. The 1-D DFT equation is:
    
    $$F(u)=\frac{1}{M}\sum_{x=0}^{M-1}f(x)e^{-j2\pi ux/M}$$
    
- **Euler's Formula**: The complex exponential is expanded using $e^{\pm j\theta}=cos(\theta)\pm jsin(\theta)$.
    <br>
- **Examples**: For a signal $f(x) = [50, 10, 30, 10]$, calculating $F(0)$ yields 25, which is exactly the average of the $f(x)$ values. Subsequent calculations for $F(1)$, $F(2)$, etc., result in complex numbers with real $R(u)$ and imaginary $I(u)$ parts (where $j = \sqrt{-1}$).
    

**2. 2-D Continuous & Discrete Signals (Slides 21-28)**

- **2-D Continuous**: An extension of 1-D concepts, utilizing double integrals.
    <br>
- **2-D Discrete (Image Processing)**: Used for digital images. The 2-D DFT equation is:
    
    $$F(u,v)=\sum_{x=0}^{M-1}\sum_{y=0}^{N-1}f(x,y)e^{-j2\pi(ux/M+vy/N)}$$
    
- **Expanded Formula**: This can be broken into Real $R(u,v)$ and Imaginary $I(u,v)$ parts using cosines and sines.
    <br>
- **Example Calculation**: For a $2 \times 2$ image matrix, the slides walk through substituting trigonometric values (like $cos(\pi)$ and $sin(\pi/2)$) to manually compute the frequency components $F(0,0)$ through $F(1,1)$.
    

---

### **Image Spectra and The Filtering Pipeline (Slides 29-36)**

- **Spectrum Characteristics**: The $F(0,0)$ value is typically the largest component of the image, while other frequencies diminish rapidly. Because the magnitude drops off so quickly, we display the spectrum using a log transformation: $log(1+|F(u,v)|)$.
    
- **The Filtering Pipeline**:
    
    1. Compute the DFT $F(u,v)$ of the input image $f(x,y)$.
        <br>
    2. Multiply $F(u,v)$ by a chosen filter function $H(u,v)$.
        <br>
    3. Compute the Inverse DFT of the result to get the enhanced image $g(x,y)$. * **Magnitude vs. Phase**: The Fourier Transform outputs a complex number, $F(u,v) = R(u,v) + jI(u,v)$.
        
    
    - **Magnitude (Amplitude)**: Calculated as $|F(u,v)|=[R^{2}(u,v)+I^{2}(u,v)]^{1/2}$. It dictates the size/height of the peak (intensities) and reveals the presence and boundaries of features.
        <br>
    - **Phase**: Calculated as $\phi(u,v)=tan^{-1}[\frac{I(u,v)}{R(u,v)}]$. It acts as a measure of displacement, encoding exactly _where_ objects are located in the image.
        <br>
    - Because phase is crucial for maintaining the spatial coherence of the image, frequency domain filtering usually only manipulates the amplitude spectrum, leaving the phase untouched.
        

---

### **Shifting & Fourier Properties (Slides 37-40)**

- **Shifting the Origin**: To make the Fourier spectrum easier to analyze (putting the low frequencies in the center), we multiply the input image $f(x,y)$ by $(-1)^{x+y}$ before applying the transform.
    <br>
- **Properties**: Operations applied in the spatial domain have direct geometric impacts in the frequency domain. The slides visually demonstrate how image rotation, linear shifting, and scaling alter the resulting Fourier spectrum pattern.