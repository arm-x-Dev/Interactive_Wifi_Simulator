# Wi-Fi RSSI Spatial Simulation Sandbox

<p align="center">
  <img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white" alt="HTML5 Badge"/>
  <img src="https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white" alt="CSS3 Badge"/>
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript Badge"/>
  <img src="https://img.shields.io/badge/GitHub_Pages-222222?style=for-the-badge&logo=github&logoColor=white" alt="GitHub Pages Badge"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="MIT License Badge"/>
</p>

Welcome to the **Wi-Fi RSSI Spatial Simulation Sandbox**. This application is a lightweight, responsive, and highly interactive physics-based spatial propagation simulator. It allows users to design rooms with walls of varying materials, position multiple Wi-Fi routers, walk a receiver client through the room, and analyze real-time signal attenuation and RSSI telemetry.

Additionally, this sandbox embeds empirical measurements from a real laboratory setup (`scan1.txt`), bridging the gap between mathematical models and real-world findings.

---

## 🌟 Key Features

* **User Onboarding Page:** An elegant glassmorphic title screen welcoming the user, outlining the key tools, and giving a quick overview of what is going to happen next.
* **Interactive Sandbox Drawing Canvas:**
  * **Edit APs:** Click to place and drag up to 5 custom Access Points. Select any AP in the sidebar to configure its transmit power (dBm) and frequency bands (2.4 GHz vs 5.0 GHz).
  * **Draw Walls with Real-time Lengths:** Draw walls dynamically. A tooltip label displaying the wall's length in **meters** floats next to the midpoint in real-time.
  * **Select and Delete Specific Walls:** Click on any segment to select it. The chosen wall is highlighted with a glowing neon cyan outline, and you can delete it using the sidebar button or by pressing **Delete / Backspace** on your keyboard.
  * **Walk Client:** Place and drag a client smartphone. View a real-time ray-cast showing which walls the signal intersects and how they attenuate signal power.
* **Live Telemetry & Graphing:** Real-time signal bar readouts and a rolling historical RSSI line chart in the sidebar update dynamically as you move the client.
* **Empirical Dataset View (scan1.txt):** Switch to "Real Scan" mode to view the 5x8 tile grid representation of a real university lab hall. Toggle between the three logged Access Points (**M21**, **Alok's A34**, and **Home 2.4GHz**) to see actual averaged measurements compared on the layout.
* **Anti-Aliased Heatmap Rendering:** Draws heat values on a small offscreen canvas (Standard: 80x50, High-Res: 160x100) and scales it up using GPU-accelerated bilinear filtering to achieve a fluid 60fps user experience.

---

## 📡 Propagation Physics

The simulator calculates signal strength (RSSI) at every pixel using the **Log-Distance Path Loss Model** and **Free Space Path Loss (FSPL)**:

$$FSPL(d) = 20 \log_{10}(d) + 20 \log_{10}(f) - 27.55$$

$$RSSI(d) = P_{tx} + G - FSPL(d) - 10 \cdot n \cdot \log_{10}(d) - \sum \text{Wall Attenuation}$$

### Parameters:
* **$P_{tx}$:** Transmit power in dBm (configurable per AP).
* **$f$:** Frequency band in MHz (2.4 GHz / 2400 MHz or 5.0 GHz / 5000 MHz).
* **$n$:** Path Loss Exponent (configurable from 1.0 to 5.0).
* **Wall Attenuation:** Deducts signal power based on intersections with line-of-sight rays:
  * **Concrete:** -12 dB (highly attenuating)
  * **Wood:** -3 dB
  * **Metal:** -20 dB (maximum shielding)
  * **Glass:** -2 dB
  *(Wall attenuation scales up by 1.5x for the 5 GHz band to represent higher attenuation at higher frequencies).*

---

## 📁 File Structure

```bash
├── index.html       # Onboarding card structure & sidebar controls
├── style.css        # Responsive glassmorphic theme styles
├── app.js           # Core physics calculations & canvas renderers
├── scan1_data.js    # Embedded raw scan1.txt dataset
├── .gitignore       # Tells Git which files to ignore
└── README.md        # This documentation file
```

---

## 🚀 How to Run Locally

Since the application uses ES6 Modules, browsers block local loads via the `file://` protocol due to CORS policies. You can serve the static files locally using Python's built-in HTTP server:

```bash
# Start a local server on port 8000
python -m http.server 8000
```

Once running, open your web browser and navigate to:
[http://localhost:8000](http://localhost:8000)

---

## 🌐 Deploying to GitHub Pages

Because the app is fully static and uses relative paths, it is 100% ready to deploy to GitHub Pages out of the box:

1. Create a public repository on GitHub.
2. Push `index.html`, `style.css`, `app.js`, `scan1_data.js`, and `.gitignore` to the `main` branch.
3. Open your repository on GitHub, go to **Settings** -> **Pages**.
4. Set the Source to **Deploy from a branch**, select `main` (or `master`), and click **Save**.
5. Your live link will be ready in a minute at `https://username.github.io/repository-name/`.
