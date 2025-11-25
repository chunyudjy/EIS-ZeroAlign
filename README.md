# EIS-ZeroAlign
A simple ipynb to compute the zero-imaginary impedance intercept for EIS data, output intercept table, shift curves to (0,0), and generate aligned Nyquist plots.



## ✨ Features

This tool is designed for batch processing Electrochemical Impedance Spectroscopy (EIS) data stored in `.txt` files:

### 🔹 1. Automatic zero-crossing detection

* Reads tab-delimited EIS files containing:
  `freq/Hz`, `Re(Z)/Ohm`, `Im(Z)/Ohm`
* Flips Im(Z) → -Im(Z)
* Locates the sign-change interval
* Linearly interpolates the exact **Re(Z) where Im(Z) = 0**
* Appends the intercept into the dataset

### 🔹 2. Negative Im(Z) removal

Ensures every curve starts at **Im ≥ 0**, improving plot consistency.

### 🔹 3. Curve alignment (shift to zero)

Each curve is translated so that its interpolated zero-crossing is moved to (0,0):

```
Re_shifted = Re - Re_intercept
Im_shifted = Im - 0
```

### 🔹 4. Output intercept table

Generates:

```
Sample | ZeroIntercept_Re(Ohm)
```

as a CSV file.

### 🔹 5. Generate aligned Nyquist plot

Plots all shifted curves together for comparison.

---

## 📦 Installation

No special packages needed except:

```bash
pip install pandas numpy matplotlib
```

---

## 📁 Input file format

Your `.txt` files must contain (tab-separated):

```
freq/Hz    Re(Z)/Ohm    Im(Z)/Ohm
1.23E+03   5.123        -2.336
...
```

Skip the header is supported.

---

## ▶️ Usage

### **Run:**

```bash
python zero_align.py
```

### **Output:**

1. `zero_intercepts.csv`
2. `shifted_plot.png`
3. Terminal output showing each intercept

---

## 🧠 How it works (algorithm)

### **1. Flip imaginary axis**

Because some EIS systems output Im(Z) < 0

```
Im_corrected = -Im
```

### **2. Detect sign change**

```
y[i] * y[i+1] < 0
```

### **3. Linear interpolation**

[
x_0 = x_1 - y_1 \cdot \frac{x_2 - x_1}{y_2 - y_1}
]

### **4. Add intercept to dataset**

With Im=0.

### **5. Remove Im < 0**

Ensures a clean curve.

### **6. Shift to zero**

[
Re' = Re - x_0
]

---

## 📊 Example Plot


<img width="864" height="699" alt="image" src="https://github.com/user-attachments/assets/063638a5-a741-4782-836c-a38dbba02a6e" />

