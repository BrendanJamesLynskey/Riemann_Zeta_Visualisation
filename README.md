# Riemann Zeta Function Visualisation

An interactive domain colouring of the Riemann zeta function, built as a single-page browser app with no server or dependencies beyond Plotly.js. Explore the critical strip, locate non-trivial zeros on the critical line, watch the pole at s = 1 spin through every hue, and see how the functional equation mirrors the left half-plane from the right.

### [Launch App](https://brendanjameslynskey.github.io/Riemann_Zeta_Visualisation/)

---

## What is this?

The **Riemann zeta function** is one of the most important objects in all of mathematics. For complex numbers s with Re(s) > 1 it is defined by the absolutely convergent Dirichlet series

$$\zeta(s) = \sum_{n=1}^{\infty} \frac{1}{n^s}$$

but its significance reaches far beyond that convergent region. Euler discovered that the zeta function encodes the distribution of prime numbers through the **Euler product**

$$\zeta(s) = \prod_{p\;\text{prime}} \frac{1}{1 - p^{-s}}, \qquad \operatorname{Re}(s) > 1$$

which equates an additive sum over all positive integers with a multiplicative product over all primes -- a bridge between addition and multiplication that lies at the heart of analytic number theory.

Riemann showed in 1859 that the zeta function can be **analytically continued** to a meromorphic function on the entire complex plane, with a single simple pole at s = 1 and no other singularities. The continuation satisfies a remarkable **functional equation**

$$\zeta(s) = 2^s \, \pi^{s-1} \, \sin\!\left(\frac{\pi s}{2}\right) \, \Gamma(1-s) \, \zeta(1-s)$$

which relates the value at s to the value at 1 - s, creating a mirror symmetry about the line Re(s) = 1/2. The functional equation forces **trivial zeros** at s = -2, -4, -6, ... (where the sine factor vanishes). All remaining zeros -- the **non-trivial zeros** -- are known to lie inside the **critical strip** 0 < Re(s) < 1, and the **Riemann Hypothesis** conjectures that every single one of them falls exactly on the **critical line** Re(s) = 1/2. This conjecture, now over 160 years old and verified computationally for more than ten trillion zeros, remains one of the seven Clay Millennium Prize Problems and is widely regarded as the most important unsolved problem in pure mathematics.

This app visualises the zeta function through **domain colouring**, a technique for plotting complex-valued functions of a complex variable. Each point s in the complex plane is coloured according to the complex number zeta(s):

- **Hue** encodes the **argument** (phase angle) of zeta(s) -- red for arg = 0, cycling through yellow, green, cyan, blue, magenta, and back to red as the argument sweeps from 0 to 2 pi. A full rainbow loop around a point means the function winds once around the origin there.
- **Brightness** encodes the **modulus** (absolute value) of zeta(s), with logarithmic contour bands so that both very large and very small values remain visible.

Zeros of the function appear as **dark spots where all colours converge** -- every hue meets at a single point because the argument is undefined at zero. Poles appear as bright spots with the same all-colours-meeting pattern, but with brightness increasing rather than decreasing. The number of times the hue cycles around a point reveals the order of the zero or pole.

## Preset Views

The app ships with six preset views, each designed to highlight a different aspect of the zeta function:

| Preset | Region | What to look for |
|--------|--------|-----------------|
| **Full Overview** | Re: [-5, 5], Im: [-30, 30] | The big picture. The critical strip runs vertically through the centre, the pole glows at s = 1, trivial zeros dot the negative real axis, and non-trivial zeros line up along Re(s) = 1/2. Notice the difference in texture between the right half-plane (where the Dirichlet series converges) and the left half-plane (computed via the functional equation). |
| **Critical Strip** | Re: [-0.5, 1.5], Im: [-35, 35] | A close-up of the strip 0 < Re(s) < 1 where all non-trivial zeros must live. The dashed blue critical line bisects the strip, and green dots mark the known zeros. Observe how the colour pattern is symmetric about the real axis (because zeta(conj(s)) = conj(zeta(s))). |
| **First Zeros** | Re: [-1, 2], Im: [10, 25] | Zooms into the first few non-trivial zeros (t ~ 14.13, 21.02, 25.01). Each zero is a dark vortex where all hues spiral inward. The spacing between zeros is irregular but statistically follows the Montgomery pair correlation conjecture. |
| **Trivial Zeros** | Re: [-12, 2], Im: [-3, 3] | The negative real axis, where the trivial zeros sit at s = -2, -4, -6, -8, -10, -12. These are forced by the sin(pi s/2) factor in the functional equation. They appear as amber dots with characteristic colour pinches. |
| **Pole at s=1** | Re: [-1, 3], Im: [-3, 3] | The only singularity of zeta. Watch the colours cycle through every hue as you circle s = 1 -- a hallmark of a simple pole with residue 1. The pink marker highlights the pole location. |
| **Gram Points** | Re: [-0.2, 1.2], Im: [28, 45] | Higher zeros in the range t ~ 28-45. The zeros become more closely spaced as t increases, consistent with the average zero density ~ (1/2 pi) log(t/2 pi e). Fine colour detail reveals the rapid oscillations of zeta along the critical line. |

## Mathematical Implementation

The zeta function cannot be computed from its defining series outside Re(s) > 1, so the app uses three interlocking algorithms:

**For Re(s) > 0: Dirichlet eta with Borwein/Knopp acceleration.** The alternating series eta(s) = sum (-1)^(n-1) / n^s converges (conditionally) for Re(s) > 0, and is related to zeta by eta(s) = (1 - 2^(1-s)) zeta(s). Raw partial sums converge too slowly, so the app applies a Knopp acceleration with precomputed binomial-weight coefficients over 40 terms, giving roughly 12 digits of accuracy across the right half-plane. The accelerated eta is then divided by (1 - 2^(1-s)) to recover zeta, with a guard for the removable singularity near s = 1.

**For Re(s) <= 0: Functional equation.** When Re(s) <= 0, the app reflects to 1 - s (which has Re > 1, safely in the convergent region) and applies the functional equation zeta(s) = 2^s pi^(s-1) sin(pi s/2) Gamma(1-s) zeta(1-s).

**Gamma function: Lanczos approximation.** The complex Gamma function is computed using the Lanczos approximation with g = 7 and 9 coefficients, providing around 15 digits of precision. For Re(z) < 0.5 the reflection formula Gamma(z) Gamma(1-z) = pi / sin(pi z) is applied first to shift into the right half-plane.

**Complex arithmetic library.** All operations -- addition, subtraction, multiplication, division, exponentiation, logarithm, sine, and the Gamma function -- are implemented from scratch in JavaScript using two-element arrays [re, im] for complex numbers. No external maths library is used.

## Features

**Domain colouring canvas**
- Renders zeta(s) on an HTML5 Canvas element, computing every pixel from the zeta function in real time
- Hue maps the argument (phase) of zeta(s); logarithmic brightness contours reveal the modulus structure
- Chunked rendering (5 rows per animation frame) keeps the browser responsive during computation
- Rendered image is computed at a configurable internal resolution and then upscaled with bilinear interpolation to fill the canvas

**Interactive overlays**
- Dashed blue line marks the **critical line** Re(s) = 1/2
- Translucent blue shading highlights the **critical strip** 0 < Re(s) < 1
- Dashed white lines mark the real and imaginary axes with auto-scaled tick labels
- Green dots mark the first 15 known **non-trivial zeros** (both +t and -t, so 30 dots total)
- Amber dots mark **trivial zeros** at s = -2, -4, ..., -12
- Pink dot and label mark the **pole** at s = 1
- Axis tick labels auto-scale to maintain readability at every zoom level

**Real-time info panel**
- Hover anywhere on the canvas to see the complex coordinate s, the value zeta(s), the modulus |zeta(s)|, the argument arg(zeta(s)) in degrees, and the distance to the nearest known non-trivial zero
- Values update on every mouse move with no perceptible lag

**Controls**
- Five range sliders for Re(s) min, Re(s) max, Im(s) min, Im(s) max, and rendering resolution (60 to 500 pixels on the short axis)
- Debounced re-render (200 ms delay) prevents wasted computation while dragging sliders
- Six preset buttons instantly jump to curated regions of the complex plane

**Critical line plot**
- A Plotly.js line chart below the main canvas plots |zeta(1/2 + it)| as a function of t over the currently visible imaginary range
- Non-trivial zeros (where the curve touches zero) are marked with green dots
- The plot updates automatically whenever the view region changes, keeping the two displays synchronised

**Colour wheel legend**
- A circular colour legend on the sidebar shows how hue maps to argument, oriented in standard mathematical convention (angle measured counter-clockwise from the positive real axis)

**Zero catalogue**
- The sidebar lists all 15 stored non-trivial zeros with their imaginary parts to 9 decimal places
- Scrollable container keeps the list compact

## Built With

- **HTML5 Canvas** for the domain colouring and colour wheel legend
- **[Plotly.js](https://plotly.com/javascript/) v2.35.0** for the interactive critical line plot
- **Pure JavaScript** -- all zeta function computation, complex arithmetic, Gamma function, and rendering logic implemented from scratch with no maths libraries
- **CSS custom properties** and a dark colour scheme for a clean, modern UI
- **GitHub Pages** for zero-configuration static hosting
- Single `index.html` file, no build step, no bundler, no frameworks
