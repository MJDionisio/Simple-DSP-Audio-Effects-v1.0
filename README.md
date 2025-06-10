# Simple-DSP-Audio-Effects-v1.0

Simple DSP Audio Effects (online) Apply effects like delay, reverb, and distortion using real-time processing. 

# 🎧 Simple DSP Audio Effects Web App

A fully browser-based Digital Signal Processing (DSP) app built with **Streamlit**, allowing users to:

- Record audio from the **microphone**
- Apply audio effects like:
  - ⏱️ Delay (with adjustable time & decay)
  - 🌊 Reverb (with adjustable decay & repeats)
  - 🔊 Distortion (adjustable gain)
- Visualize original and processed audio waveforms
- Play & download the processed audio

---

## 👥 Project Information

**Subject**: Digital Signal Processing (Lab)  
**Professor**: Engr. Jonathan V. Taylar  
**Group Members**:
- Barroga, Bradley  
- Dionisio, Michael Jordan S.  
- FredLaminoza, Razel  
- Setarios, Christian

---

## 🧪 Live Demo

> [Launch the App on Streamlit Cloud](https://simple-dsp-audio-effects-v1-app.streamlit.app/)

---

## 💡 Features

✅ Browser-based microphone recording using `st.audio_input()`  
✅ Real-time DSP effect processing with interactive controls  
✅ Waveform visualization using `matplotlib`  
✅ Audio download button after applying effects  
✅ Auto-detects sample rate and warns on long/short recordings  

---

## 📂 File Structure

```bash
📁 simple-dsp-audio-effects/
│
├── main.py              # Streamlit app
├── requirements.txt     # Python dependencies
└── README.md            # This file
