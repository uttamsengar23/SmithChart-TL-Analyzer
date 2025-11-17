
#  **DOCUMENTATION**

# **Smith Chart Based Transmission Line Analyzer (MATLAB App)**

# **1. Introduction**

This MATLAB App Designer project provides an interactive environment for analyzing the behavior of high-frequency transmission lines. It performs essential RF computations, visualizes impedance and admittance transformations, and plots both domains on a custom-rendered Smith Chart.

The tool is built to support:

* Reflection Coefficient at Load (Γₗ)
* Input Reflection Coefficient (Γᵢₙ)
* Standing Wave Ratio (SWR)
* Input Impedance (Zᵢₙ)
* Load Admittance (Yₗ)
* Input Admittance (Yᵢₙ)
* Transmission line transformation using electrical length (βl)

The Smith Chart visualization includes:

* Unit Circle
* SWR Circle
* Impedance Points (Γₗ, Γᵢₙ)
* Admittance Points (−Γₗ, −Γᵢₙ)

This makes the app valuable for RF engineering students, microwave engineers, antenna designers, and researchers working with matching networks.

---

# **2. Fundamentals of Transmission Lines**

Transmission lines at RF frequencies behave as distributed networks, not simple wires. Their per-unit-length elements cause voltage and current to vary sinusoidally along their length.

The primary distributed parameters are:

* R — Series resistance
* L — Series inductance
* G — Shunt conductance
* C — Shunt capacitance

These parameters lead to the telegrapher’s equations, which describe wave propagation and impedance transformation.

---

# **3. Characteristic Impedance (Z₀)**

The characteristic impedance is given by:

```
Z0 = sqrt((R + jωL) / (G + jωC))
```

For low-loss lines:

```
Z0 ≈ sqrt(L / C)
```

In the app, Z₀ is entered through:

```matlab
Z0 = app.Z0EditField.Value;
```

---

# **4. Load Impedance (ZL)**

The user enters the load in complex form:

```
ZL = RL + jXL
```

Examples:

```
25+30i
100 - j50
3+4*i
```

MATLAB conversion:

```matlab
ZL = str2num(ZL_str);
```

---

# **5. Reflection Coefficient at Load (Γₗ)**

Defined as:

```
Gamma_L = (ZL - Z0) / (ZL + Z0)
```

Its magnitude indicates mismatch strength, and its angle indicates phase.

---

# **6. Standing Wave Ratio (SWR)**

```
SWR = (1 + |Gamma|) / (1 - |Gamma|)
```

An SWR of 1 indicates perfect matching.

---

# **7. Electrical Length (βl)**

The app accepts electrical length directly:

```
beta*l
```

Examples:

```
0.5
pi/4
1.57
```

This value represents the phase shift along the transmission line.

---

# **8. Input Impedance (Zᵢₙ)**

Based on the transmission line input impedance formula:

```
Zin = Z0 * (ZL + jZ0 tan(βl)) / (Z0 + jZL tan(βl))
```

This expresses how the line transforms the load.

---

# **9. Input Reflection Coefficient (Γᵢₙ)**

Once Zᵢₙ is known:

```
Gamma_in = (Zin - Z0) / (Zin + Z0)
```

This point is plotted on the Smith Chart.

---

# **10. Admittance Representation**

Admittance is the reciprocal of impedance:

```
Y = 1 / Z
```

Two admittances are computed:

* Load Admittance:

  ```
  Y_L = 1 / ZL
  ```

* Input Admittance:

  ```
  Y_in = 1 / Zin
  ```

Both values are displayed in Siemens (S).
The imaginary part shows susceptance, identifying capacitive (−jB) or inductive (+jB) nature.

---

# **11. Admittance Reflection Coefficient**

In the Smith Chart, admittance is handled using:

```
Gamma_Y = -Gamma_Z
```

This produces a 180° rotation of the impedance point across the origin.

Thus:

* Load Admittance Reflection Coefficient: −Γₗ
* Input Admittance Reflection Coefficient: −Γᵢₙ

These are plotted with green markers for clear distinction.

---

# **12. Smith Chart Rendering Approach**

The chart uses the Γ-plane representation:

* Unit Circle: |Γ| = 1
* SWR Circle: |Γₗ|
* Γₗ → red star
* Γᵢₙ → blue circle
* −Γₗ → green triangle
* −Γᵢₙ → green circle

All circles and points are manually drawn using cosine–sine parametric curves and line primitives. Axis scaling is locked to maintain circular geometry.

---

# **13. GUI Components**

### **Inputs**

* Z₀ (Ohms)
* ZL (complex)
* Line Length (βl)

### **Outputs**

* SWR
* Gamma (magnitude and angle)
* Z_in
* Input Admittance (Yᵢₙ)
* Load Admittance (Yₗ)

### **Visualization**

A full Smith Chart showing both impedance and admittance domains simultaneously.

---

# **14. Example Analysis and Interpretation**

Using the inputs:

```
Z0 = 50 Ω
ZL = 3 + 4i Ω
βl = 0 rad
```

The app computes:

### **Reflection Coefficient**

```
Γ_L = 0.887 ∠ 170.82°
```

### **SWR**

```
SWR = 16.774
```

### **Input Impedance**

```
Z_in = 3 + j4 Ω
```

### **Load and Input Admittance**

```
Y_L = 0.1200 – j0.1600 S
Y_in = 0.1200 – j0.1600 S
```

These values align with high mismatch (Γ close to 1) and strong capacitive susceptance (negative imaginary part).

---

# **15. Smith Chart Illustration and Explanation**

The figure below shows the full impedance–admittance visualization produced for the example:
![Smith Chart Illustration](https://raw.githubusercontent.com/uttamsengar23/SmithChart-TL-Analyzer/refs/heads/main/smitchartnew.png)


### **Explanation:**

1. **White Dotted Circle**
   Represents |Γ| = 1, the fundamental Smith Chart boundary.

2. **Red Dashed Circle**
   SWR circle corresponding to |Γₗ| = 0.887.

3. **Red Star (Γₗ)**
   Load reflection coefficient located near the boundary, indicating heavy mismatch.

4. **Blue Circle (Γᵢₙ)**
   For βl = 0, Γᵢₙ overlaps exactly with Γₗ.

5. **Green Triangle (−Γₗ)**
   Admittance reflection coefficient of the load.

6. **Green Circle (−Γᵢₙ)**
   Input admittance reflection coefficient.

7. **Grid, Axes, Legend**
   Improve readability and clarity of the Γ-plane.

This dual representation provides clear insight into both impedance and admittance behavior of the line.

---









