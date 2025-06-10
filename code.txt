import streamlit as st
import numpy as np
import soundfile as sf
import io
import matplotlib.pyplot as plt

st.set_page_config(page_title="Simple DSP Audio Effects", layout="centered")

st.title("🎧 Simple DSP Audio Effects v1.0")
st.subheader("(GROUP 6) - Project in Digital Signal Processing - Lab")

names = {
    "Members": [
        "Barroga, Bradley",
        "Dionisio, Michael Jordan S.",
        "FredLaminoza, Razel",
        "Setarios, Christian"
    ]
}
st.json(names, expanded=True)
st.write("Professor : Engr. Jonathan V. Taylar")

# 🎤 Record audio
audio_file = st.audio_input("🎙️ Record your voice below:")

# Only show effects after recording
if audio_file:
    audio_bytes = audio_file.read()
    data, samplerate = sf.read(io.BytesIO(audio_bytes))

    duration_sec = len(data) / samplerate
    st.success(f"✅ Recorded {duration_sec:.2f} seconds at {samplerate} Hz")

    if duration_sec < 1:
        st.warning("⚠️ Recording too short. Speak a bit longer.")
    elif duration_sec > 20:
        st.warning("⚠️ Long recording — processing may take longer.")

    st.subheader("🔊 Original Audio")
    st.audio(audio_bytes, format="audio/wav")

    # 📈 Plot waveform
    st.subheader("📉 Waveform")
    fig, ax = plt.subplots()
    ax.plot(np.linspace(0, duration_sec, len(data)), data, color='blue')
    ax.set_xlabel("Time (s)")
    ax.set_ylabel("Amplitude")
    ax.set_title("Original Audio Waveform")
    st.pyplot(fig)

    # 🎛 Select effect
    st.subheader("🎛️ Choose DSP Effect")
    effect = st.radio("Effect:", ["Normal", "Delay", "Reverb", "Distortion"])

    # Show controls based on effect
    if effect == "Delay":
        delay_time = st.slider("Delay Time (sec)", 0.05, 1.0, 0.3, 0.05)
        delay_decay = st.slider("Decay", 0.0, 1.0, 0.5, 0.1)
    elif effect == "Reverb":
        reverb_decay = st.slider("Reverb Decay", 0.0, 1.0, 0.4, 0.1)
        reverb_repeats = st.slider("Reverb Repeats", 1, 10, 5)
    elif effect == "Distortion":
        gain = st.slider("Distortion Gain", 1, 30, 10)

    # 🎚 DSP Functions
    def apply_delay(audio, delay_sec=0.3, decay=0.5):
        delay_samples = int(delay_sec * samplerate)
        buffer = np.zeros(len(audio) + delay_samples)
        buffer[:len(audio)] = audio
        for i in range(len(audio)):
            buffer[i + delay_samples] += decay * audio[i]
        return buffer[:len(audio)]

    def apply_reverb(audio, decay=0.4, repeats=5):
        result = np.copy(audio)
        for i in range(1, repeats):
            delay = int(i * 0.05 * samplerate)
            padded = np.pad(audio, (delay, 0), 'constant')[:len(audio)]
            result += decay**i * padded
        return result

    def apply_distortion(audio, gain=10):
        return np.tanh(gain * audio)

    # 🔁 Apply effect
    if st.button("🎶 Apply Effect"):
        if effect == "Delay":
            processed = apply_delay(data, delay_time, delay_decay)
        elif effect == "Reverb":
            processed = apply_reverb(data, reverb_decay, reverb_repeats)
        elif effect == "Distortion":
            processed = apply_distortion(data, gain)
        else:
            processed = data

        # Save to buffer
        output_buffer = io.BytesIO()
        sf.write(output_buffer, processed, samplerate, format='WAV')
        output_buffer.seek(0)

        # Play result
        st.subheader(f"🎧 Processed Audio: {effect}")
        st.audio(output_buffer, format="audio/wav")
        st.download_button("⬇️ Download Processed Audio", output_buffer, file_name="processed.wav")

        # 📈 Show processed waveform
        st.subheader("📉 Processed Waveform")
        fig2, ax2 = plt.subplots()
        ax2.plot(np.linspace(0, len(processed)/samplerate, len(processed)), processed, color='green')
        ax2.set_xlabel("Time (s)")
        ax2.set_ylabel("Amplitude")
        ax2.set_title("Processed Audio Waveform")
        st.pyplot(fig2)

st.write("Created by: MJ S. Dionisio")
