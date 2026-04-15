# GNSS Security Evalutaion on Android

![Polito Logo](logo_polito.jpg)

This repository contains an academic report and technical analysis focused on the security vulnerabilities and physical reliability of Global Navigation Satellite Systems (GNSS) on commercial Android devices. The work was developed as part of the Wireless Security course within the Master's degree in Cybersecurity Engineering at **Politecnico di Torino (PoliTo)**.

## Project Overview
The primary objective of this project is to evaluate how commercial GNSS receivers behave under various degrees of physical signal obstruction and during simulated **cyber-spoofing attacks**. Using raw Android measurements, the study identifies hardware vulnerabilities and demonstrates the effectiveness of data processing in detecting signal anomalies. 

## Experimental Scenarios
Data was collected using Google's *GNSSLogger* app across five distinct environmental conditions to establish baseline performances and interference limits:
* **Static (Open Sky):** Ideal baseline with optimal line-of-sight and steady C/N0 levels.
* **Dynamic (Pedestrian & Bus):** Evaluation of multipath interference, velocity spikes, and signal fading in urban canyons.
* **Body Interference:** Analysis of signal degradation caused by human tissue absorption in the GNSS L-bands.
* **Faraday Cage Simulation (Aluminum Shielding):** Severe signal deprivation and phase noise induced by partial metallic shielding, highlighting rapid HDOP spikes and satellite tracking losses.

## Cyber-Spoofing Vulnerability Analysis
The logs were manipulated using MATLAB to inject falsified coordinates and temporal anomalies. The analysis covers a matrix of four distinct attack vectors:
1. **Near Receiver (No Delay):** A stealthy attack producing a small geometric shift, virtually imperceptible in raw pseudoranges but exposing a sudden jump in the Weighted Least Squares (WLS) position states.
2. **Near Receiver (With 4ms Delay):** Introduces a massive vertical step across all pseudoranges. The WLS solver completely absorbs this delay into the hardware clock bias, leaving the spatial coordinates relatively untouched but severely compromising timing synchronization.
3. **Far From Receiver (With Delay):** A worst-case scenario (478 km shift). Produces aggressive discontinuities in pseudorange rates, position states, and a violent spike in the clock bias.
4. **Far From Receiver (No Delay):** The solver interprets this strictly as an instantaneous spatial teleportation. The position jumps by an order of magnitude of 100,000 meters, but the hardware clock smoothly follows its natural continuous drift.

## Key Metrics Monitored
* **Carrier-to-Noise Ratio (C/N0):** Used to detect physical signal blockages and as a primary indicator for software-based spoofing (since simulated attacks leave C/N0 unchanged).
* **Horizontal Dilution of Precision (HDOP):** Tracking the deterioration of satellite constellation geometry during signal loss.
* **Hardware Clock Bias & Pseudorange Rates (Doppler):** Crucial parameters for identifying sophisticated positional and temporal anomalies.

## Tools & Methodology
* **Data Logging:** Google GNSSLogger (Android)
* **Data Processing & Spoofing:** MATLAB (Google GPS Measurement Tools)

---
*Disclaimer: This repository is for academic and educational purposes only. The spoofing attacks analyzed are strictly software-based simulations (data manipulation) and do not involve illegal over-the-air RF transmissions.*
