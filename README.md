# Riemann Zeta Function Visualisation

**[Launch App](https://brendanjameslynskey.github.io/Riemann_Zeta_Visualisation/)**

An interactive domain colouring of the Riemann zeta function, built as a single-page browser app. Explore the critical strip, locate non-trivial zeros, and see the functional equation in action.

## About the Riemann Zeta Function

The Riemann zeta function is defined for complex numbers s with Re(s) > 1 by the Dirichlet series:

    zeta(s) = sum_{n=1}^{infinity} 1/n^s

and extended to the entire complex plane (except for a simple pole at s = 1) by analytic continuation. The **Riemann Hypothesis** -- one of the most famous unsolved problems in mathematics -- conjectures that all non-trivial zeros of zeta(s) lie on the **critical line** Re(s) = 1/2.

This visualisation uses domain colouring to map the complex output of zeta(s) to colour: hue encodes the argument (phase) and brightness encodes the modulus, with logarithmic contour lines revealing the function's structure.

## Preset Views

| Preset | Region | What to look for |
|--------|--------|-----------------|
| **Full Overview** | Re: -5 to 5, Im: -30 to 30 | Global structure, critical strip, pole |
| **Critical Strip** | Re: -0.5 to 1.5, Im: -35 to 35 | The strip 0 < Re(s) < 1 where all non-trivial zeros live |
| **First Zeros** | Re: -1 to 2, Im: 10 to 25 | First few non-trivial zeros on Re(s) = 1/2 |
| **Trivial Zeros** | Re: -12 to 2, Im: -3 to 3 | Zeros at s = -2, -4, -6, ... on the real axis |
| **Pole at s=1** | Re: -1 to 3, Im: -3 to 3 | The simple pole, visible as colour cycling around s = 1 |
| **Gram Points** | Re: -0.2 to 1.2, Im: 28 to 45 | Higher zeros region with fine detail |

## Features

- Domain colouring rendered on HTML5 Canvas with hue = arg(zeta(s)) and log-scale contour lines
- Adjustable view region with sliders and preset views
- Critical line Re(s) = 1/2 shown as a dashed overlay
- Critical strip 0 < Re(s) < 1 highlighted with subtle shading
- First 15 known non-trivial zeros marked as dots
- Trivial zeros and pole at s = 1 annotated
- Real-time info panel showing zeta(s), |zeta(s)|, arg(zeta(s)), and distance to nearest known zero at mouse position
- Side plot of |zeta(1/2 + it)| vs t showing zeros as dips
- Adjustable resolution (trade speed for quality)
- Colour wheel legend for phase interpretation

## Mathematical Implementation

- **Re(s) > 0**: Dirichlet eta function with Borwein/Knopp acceleration (40 terms), then zeta(s) = eta(s) / (1 - 2^(1-s))
- **Re(s) <= 0**: Functional equation zeta(s) = 2^s pi^(s-1) sin(pi s/2) Gamma(1-s) zeta(1-s)
- **Gamma function**: Lanczos approximation with reflection formula
- **Complex arithmetic**: Full library for exp, log, pow, sin, gamma on complex numbers

## Built With

- HTML5 Canvas for domain colouring
- [Plotly.js](https://plotly.com/javascript/) for the critical line plot
- Pure JavaScript -- no server required
- GitHub Pages for hosting
