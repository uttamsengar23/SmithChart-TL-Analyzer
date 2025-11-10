#  **DOCUMENTATION**

## **Smith Chart Based Transmission Line Analyzer (MATLAB App)**

# **1. Introduction**

This MATLAB App Designer project is an **interactive visualization tool** designed to help users understand the behavior of **high‑frequency transmission lines**. It implements the following key RF computations:

* Reflection Coefficient at Load (Gamma_L)
* Input Reflection Coefficient (Gamma_in)
* Standing Wave Ratio (SWR)
* Input Impedance (Z_in)
* Impedance transformation using electrical length (beta*l)

The app includes a **custom‑drawn Smith Chart** that displays:

* Unit Circle (|Gamma| = 1)
* SWR Circle (|Gamma_L|)
* Load Reflection Point (red star)
* Input Reflection Point (blue circle)

Its purpose is to provide intuitive learning for:

* RF Students
* Microwave Engineers
* Amateur Radio / Antenna Designers
* Researchers working with transmission lines and matching networks

---

#  **2. Fundamentals of Transmission Lines**

Transmission lines at RF cannot be modeled as simple wires. They behave as **distributed systems**, where voltage and current vary continuously along length.

Transmission lines behave like:

* Distributed inductance (L)
* Distributed capacitance (C)
* Distributed resistance (R)
* Distributed dielectric leakage (G)

This leads to wave propagation governed by the telegrapher’s equations.

---

#  **3. Characteristic Impedance (Z0)**

The characteristic impedance determines the V/I ratio of a forward‑traveling wave.

```
Z0 = sqrt( (R + j·ω·L) / (G + j·ω·C) )
```

Where:

| Symbol | Meaning                                |
| ------ | -------------------------------------- |
| R      | Series resistance per unit length      |
| L      | Inductance per unit length             |
| G      | Dielectric conductance per unit length |
| C      | Capacitance per unit length            |

For low‑loss lines:

```
Z0 ≈ sqrt(L / C)
```

### Code tie‑in:

```matlab
Z0 = app.Z0EditField.Value;
```

---

#  **4. Load Impedance (ZL)**

The load attached at the end of the line is:

```
ZL = RL + j·XL
```

The app accepts complex input strings like:

```
25+30i
100 - j50
(50 + j20)
```

MATLAB converts this text into a complex number:

```matlab
ZL = str2num(ZL_str);
```

---

#  **5. Reflection Coefficient (Gamma_L)**

A mismatch (ZL ≠ Z0) causes reflections.

```
Gamma_L = (ZL - Z0) / (ZL + Z0)
```

### Meaning:

* |Gamma| → Strength of reflection
* Angle(Gamma) → Phase shift of reflected wave
* Gamma = 0 → Perfect match
* |Gamma| = 1 → Total reflection (open/short)

### Code:

```matlab
Gamma = (ZL - Z0) / (ZL + Z0);
```

The app displays:

* Magnitude
* Angle (degrees)
* Position on Smith chart

---

#  **6. Standing Wave Ratio (SWR)**

SWR measures the severity of mismatch.

```
SWR = (1 + |Gamma|) / (1 - |Gamma|)
```

### Code:

```matlab
SWR = (1 + abs(Gamma)) / (1 - abs(Gamma));
```

The SWR circle is drawn with radius = |Gamma|.

---

#  **7. Electrical Length (beta·l)**

Instead of requiring frequency and physical length separately, the app takes **electrical angle directly in radians**.

```
beta = 2π / λ
beta·l = electrical phase shift
```

User inputs:

```
0.5
pi/4
1.57
```

### Code:

```matlab
beta_l = l;
```

---

#  **8. Input Impedance of a Transmission Line**

Impedance transforms as signals travel along a line:

```
Zin = Z0 * (ZL + j·Z0·tan(beta·l)) / (Z0 + j·ZL·tan(beta·l))
```

This equation is central to RF/microwave engineering.

### Code:

```matlab
numerator = ZL + 1i*Z0*tan(beta_l);
denominator = Z0 + 1i*ZL*tan(beta_l);
Zin = Z0 * (numerator / denominator);
```

### GUI output example:

```
Z_in (Ohms): 45.00 + j(-22.50)
```

---

#  **9. Input Reflection Coefficient (Gamma_in)**

Once Zin is known:

```
Gamma_in = (Zin - Z0) / (Zin + Z0)
```

### Code:

```matlab
Gamma_in = (Zin - Z0) / (Zin + Z0);
```

Gamma_in is plotted as the **blue circle**.

---

#  **10. Smith Chart Theory (Gamma‑Plane Rendering)**

The Smith chart plots:

```
Gamma = x + j·y
```

with:

```
|Gamma| ≤ 1
```

The app uses a **Gamma‑plane representation**, drawing:

* Unit circle (dotted)
* SWR circle (dashed red)
* Load point (red star)
* Input point (blue circle)

### Unit circle code:

```matlab
plot(ax, cos(theta), sin(theta), ':')
```

### Plotting Gamma_L:

```matlab
plot(real(Gamma), imag(Gamma), 'r*')
```

### Plotting Gamma_in:

```matlab
plot(real(Gamma_in), imag(Gamma_in), 'bo')
```

This approach is mathematically valid and visually intuitive.

---

#  **11. MATLAB GUI Architecture**

Your App Designer GUI consists of:

###  **Input Components**

* `NumericEditField` for Z0
* `TextEditField` for ZL (complex)
* `TextEditField` for beta·l

###  **Output Labels**

Dynamic updates:

```matlab
app.SWRLabel.Text = ...
app.ZinLabel.Text = ...
app.GammaLabel.Text = ...
```

###  **Axes Rendering**

The Smith chart is built from:

* Line primitives
* Parametric cosine–sine circles
* Equal‑axis scaling
* Legend overlays

This ensures full control of the visualization.

---

# **12. Why This Project Stands Out**

* Complete RF theory implementation
* True impedance transformation demonstration
* Custom Smith Chart rendering (no toolbox required)
* Accurate mapping of theory → actual MATLAB code
* Great for GitHub portfolio and academic submissions
* Highly educational visual representation

---

#  **13. Example Input & Visual Explanation**

This section demonstrates how the app processes real input values using the example below.

**User Inputs:**

```
Z0 = 50 ohms
ZL = 3 + 4i ohms
Line Length (beta*l) = 0 radians
```

---

##  **Step-by-Step Explanation of the Output**

### **1️⃣ Load Reflection Coefficient (Gamma_L)**

Using:

```
Gamma_L = (ZL - Z0) / (ZL + Z0)
```

Substitute values:

```
ZL = 3 + 4j
Z0 = 50
```

Compute numerator:

```
ZL - Z0 = (3 + 4j) - 50 = -47 + 4j
```

Compute denominator:

```
ZL + Z0 = (3 + 4j) + 50 = 53 + 4j
```

Compute Gamma:

```
Gamma_L = (-47 + 4j) / (53 + 4j)
```

Magnitude:

```
|Gamma_L| ≈ 0.887
```

Angle (deg):

```
∠Gamma ≈ 12.9°
```

The app displays this as:

```
Gamma: 0.887 < 12.9°
```

---

### **2️⃣ Standing Wave Ratio (SWR)**

```
SWR = (1 + |Gamma|) / (1 - |Gamma|)
```

Substitute:

```
SWR = (1 + 0.887) / (1 - 0.887)
     = 1.887 / 0.113
     ≈ 16.77
```

Displayed as:

```
SWR: 16.774
```

---

### **3️⃣ Input Impedance (Z_in)**

Because beta*l = 0 radians:

```
Zin = ZL    (no transformation)
```

So:

```
Zin = 3 + 4j ohms
```

Displayed as:

```
Z_in (Ohms): 3.00 + j(4.00)
```

---

### **4️⃣ Smith Chart Interpretation**

With the inputs:

* **Gamma_L** is plotted at a magnitude of ~0.887 → very close to edge.
* SWR circle radius ≈ **0.887** is drawn (red dashed).
* Input Gamma equals Load Gamma (because line length = 0).
* Blue (input) and red (load) points overlap.

This matches the displayed chart in the image.

---

#  **14. Example Demonstration (with Image)**

Below is the captured output of the app for:

```
Z0 = 50 ohms
ZL = 3 + 4i ohms
beta*l = 0
```

![Example Result](https://raw.githubusercontent.com/uttamsengar23/SmithChart-TL-Analyzer/refs/heads/main/SmithChartdemo.png)

**Explanation of the Visualization:**

* The **white dotted circle** shows |Gamma| = 1.
* The **red dashed circle** is the SWR circle for |Gamma| = 0.887.
* The **red star** marks the load reflection coefficient (Gamma_L).
* The **blue circle** marks the input reflection coefficient (Gamma_in).
  Since the line length is zero, both points overlap.

This example clearly demonstrates how the app processes RF inputs and plots the reflection behavior on a Smith Chart.

---

# End of Documentation








