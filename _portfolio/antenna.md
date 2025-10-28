---
title: "☄️ Meteor Detection Using Forward Scatter Radar"
description: "Designing a Yagi antenna and SDR-based system to detect meteors via radio reflection and Doppler analysis."
---
This project demonstrates how to use software-defined radio (SDR) and a custom-built 3-element Yagi antenna to detect meteors through **forward scatter radar**. The system is relatively straightforward to implement, and yielded phenomenal results
The system leverages the reflection of VHF signals from ionized meteor trails to observe meteor activity that is otherwise invisible to optical methods.

---

##  Concept Overview
When a meteor enters Earth’s atmosphere, it ionizes surrounding air molecules, leaving a brief trail of free electrons.  
These ionized trails reflect radio signals—particularly in the VHF range (30–300 MHz)—which can be detected even when the meteor is below the horizon.

In a **forward scatter setup**, a distant transmitter and receiver are positioned far enough apart that the transmitter’s signal is normally blocked by Earth’s curvature.  
Only when a meteor trail forms between them does a signal “bounce” from the transmitter to the receiver, creating a short-lived spike in radio intensity.

---

## System Design

### the transmitter
- Used an existing French space-surveillance radar known as **GRAVES**, which transmits a continuous wave (CW) signal at **143.05 MHz**.  
- This frequency is ideal for meteor detection because it provides strong reflections from ionized trails without atmospheric absorption.

### Antenna Design
A **3-element Yagi-Uda antenna** was designed and built using copper tubing and a wooden boom.

**Key features:**
- Reflector, driven element (dipole), and director tuned near half the 2.1 m wavelength.  
- Elements spaced between 0.1–0.25 λ for optimal gain.  
- Connected to the SDR receiver via a coaxial cable and BNC connector.

The antenna was oriented southeast (toward the transmitter) and tilted slightly upward to match typical meteor altitudes (~90 km).

*Example schematic of antenna dimensions:*  
![](../images/yagi_diagram.png)

---

## 🧰 Software Configuration
The receiver used a **RTL-SDR dongle** connected to a laptop running **SDR# (SDRSharp)**.  
- Waterfall plots display signal intensity over time.  
- A screen capture tool recorded data every 15 s to allow continuous monitoring during the **Taurid** and **Leonid** meteor showers (Nov 5–17, 2022).

---

## 📊 Exploratory Data Analysis
Hourly meteor detections were manually counted from the SDR waterfall plots.  
Meteors were classified as **underdense** (short, faint echoes) or **overdense** (longer, brighter reflections).  
A clear daily pattern emerged: detection rates peaked during the **early morning hours**, when Earth’s rotation faces the receiver into the direction of orbital motion.

![](../images/meteor_rates.png)

---

## ⚙️ Doppler Analysis
Some overdense trails exhibited distinct frequency shifts corresponding to **head echoes**, where reflections occur directly from the meteor body rather than the trail.

The **radial velocity** of such meteors was calculated using:

\[
u = \frac{c \, \Delta f}{2 f_0}
\]

For example, a Δf = 800 Hz shift yielded a velocity of **~839 m/s**, consistent with expected meteor speeds.

---

## 💡 Findings
- Detected meteors exhibited speeds between **11 – 72 km/s**.  
- Daily detection peaks aligned with the **Taurid** meteor shower period.  
- Both underdense and overdense trail types were successfully observed.  
- The system validated that low-cost SDRs can detect and analyze atmospheric ionization events.

---

## 🚀 Future Improvements
- Automate detection using **SpectrumLab** or Python-based FFT analysis instead of manual screen captures.  
- Use multiple synchronized SDR receivers for **triangulation** and height estimation.  
- Implement frequency calibration to correct for SDR offset drift.  
- Expand beyond meteors — detect satellites, aircraft, or auroras.

---

## 🧩 Tools & Components
| Component | Description |
|------------|-------------|
| **Hardware** | RTL-SDR USB receiver, 3-element Yagi-Uda antenna |
| **Software** | SDR#, Auto Screen Capture |
| **Transmitter** | GRAVES (143.05 MHz) |
| **Languages** | Python (EDA), MATLAB (signal visualization) |
| **Environment** | University of Kent experimental setup |

---

## 💬 Reflection
T
