---
title: "Meteor Observation by Forward Scatter Radar"
date: 2023-04-23
categories:
  - project
tags:
  - physics
  - signal-processing
  - sdr
  - radar
header:
  teaser: /images/article_covers/meteorobs-schematic.png
excerpt: "A hands-on exploration of meteor detection using a custom-built Yagi antenna and a software-defined radio system."
---


### Overview
This project explores how meteors can be detected using **forward scatter radar** — a method that captures radio reflections from ionized trails left by meteors entering Earth’s atmosphere. The system was built using a **Yagi antenna** and an **SDR (Software Defined Radio)** receiver tuned to the GRAVES transmitter frequency at 143.05 MHz.

### Motivation
After radar’s use in World War II, researchers discovered that meteors produced “false echoes” on radar screens. These reflections inspired a new way of observing meteor activity — by detecting their ionized trails through radio signals rather than optical telescopes. The project aimed to recreate this phenomenon with affordable, open-source tools.

---

### System Design

#### Antenna Construction
The antenna used was a **Yagi-Uda** design optimized for 143.05 MHz (λ = 2.097 m). Its copper elements were cut and mounted on a wooden boom to ensure non-conductivity and stability.  
The setup included:
- **Reflector**, **Dipole**, and **Director** elements to enhance forward gain.
- **Coaxial connection** to the SDR dongle for signal capture.
- Orientation toward **GRAVES** in France, with calculated elevation for optimal reflection angles.

#### Software Setup
Signal processing was performed in **SDRsharp**, displaying intensity over time as a *waterfall plot*. Automated screen captures every 15 seconds enabled continuous observation from November 5–17. Meteor echoes were later classified manually as **underdense** or **overdense** based on signal shape and duration.

---

### Results

#### Echo Classification
- **Underdense trails**: Faint, short-lived reflections (<1 s), often appearing as thin horizontal lines on the waterfall diagram.  
- **Overdense trails**: Brighter and longer-lasting (up to 2 s), with diffraction-like signal fluctuations due to interference between reflection points.

Head echoes — reflections from the plasma surrounding the meteor head — were occasionally detected, allowing direct velocity calculations. A Doppler shift of 800 Hz, for example, corresponded to a **radial velocity of 838.8 m/s**.

#### Detection Patterns
Detection rates peaked around **November 11–12**, coinciding with the **Taurid meteor shower**. Most echoes appeared in the early morning hours, consistent with the Earth’s rotation — when the observation region faces the orbital direction, increasing meteor entry rates.

---

### Discussion
The results confirm that forward scatter radar can successfully detect meteor trails with relatively simple equipment.  
Signal characteristics matched theoretical expectations, with observed Doppler shifts corresponding to typical meteor velocities (11–72 km/s). Limitations were mainly due to SDRsharp’s resolution and manual analysis — future improvements could include **automated image detection** and **machine learning–based signal classification**.

---

### Conclusion
This experiment demonstrates that meaningful meteor observations are achievable using low-cost SDR hardware. Beyond its technical outcomes, it shows how data-driven methods can reveal invisible celestial phenomena — translating radio noise into evidence of high-speed interplanetary travel.

---

**Keywords:** SDR, Radio Astronomy, Meteor Detection, Doppler Shift, Antenna Design  
**Tools:** SDRsharp, Auto Screen Capture, Python (for data post-processing)
