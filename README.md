# Speech Signal Analysis and Feature Extraction

## 📌 Project Overview
The assignment consists of four interrelated parts implemented within a single unified Jupyter Notebook:
1. **Speech Recording & Data Preparation:** Building a structured database from recorded human vowels and provided synthetic speech files.
2. **Formant Extraction with AI Assistance:** Using a signal-processing pipeline assisted by generative AI to estimate vocal tract resonant frequencies (F1 and F2).
3. **Formant Space Visualization:** Mapping acoustic characteristics into a 2D geometric space to evaluate vowel clustering and phonetic similarity.
4. **Pitch Estimation & Spectrogram Analysis:** Applying the `pYIN` algorithm to trace fundamental frequency ($F_0$) contours overlaid on STFT-computed log-spectrograms.

---

## 📁 Repository Structure
Your submission directory should be organized as follows:
```text
.
├── Assignment-1.ipynb   # Main Jupyter Notebook containing code and analysis
├── README.md                       # Documentation file (This file)
├── student/                        # Directory containing your 11 natural speech recordings
│   ├── <STUDENT-ID>-had.wav
│   ├── <STUDENT-ID>-hard.wav
│   └── ... (remaining .wav files)
├── synthetic1/                     # Provided synthetic speaker dataset 1
├── synthetic2/                     # Provided synthetic speaker dataset 2
├── student_concatenated.wav        # Generated combined audio for the student speaker
├── synthetic1_concatenated.wav     # Generated combined audio for synthetic group 1
└── synthetic2_concatenated.wav     # Generated combined audio for synthetic group 2

```

---

## 🛠️ Environment Setup & Dependencies

To execute the notebook and reproduce the experimental results, ensure you have Python 3.8+ installed along with the required signal processing and data analysis packages.

### Installation

You can install all necessary libraries via `pip`:

```bash
pip install numpy pandas matplotlib librosa soundfile scipy

```

### Core Libraries Used

* **`librosa`**: Used for audio loading, resampling, pre-emphasis, Short-Time Fourier Transform (STFT), LPC modeling, and the implementation of the `pYIN` pitch estimation algorithm.
* **`soundfile`**: Used for export operations to write concatenated NumPy arrays back into uncompressed `.wav` files.
* **`pandas`**: Used to construct, manipulate, and query the central experimental database tracking `speaker`, `word`, `F1`, and `F2` features.
* **`matplotlib`**: Used to generate high-resolution, publication-ready scatter plots and overlaid time-frequency graphs.

---

## 🚀 Execution Guide

1. Place the `student/` directory (containing your 11 recorded files), `synthetic1/`, and `synthetic2/` folders in the same directory as the `Assignment-1.ipynb` file.
2. Open your terminal and start the Jupyter environment:
```bash
jupyter notebook

```


3. Open `COMP3040J-Assignment-1.ipynb`.
4. To ensure full reproducibility without sequence breaks, select **Kernel -> Restart & Run All** from the top menu.

---

## 🔬 Implementation & Methodological Details

### 1. Data Preparation

* Audio signals are validated to be single-word utterances formatted as Mono, uncompressed PCM `.wav` at a sampling rate of 44.10 kHz.
* A regex-driven parser handles file discovery and updates a centralized Pandas DataFrame by matching structural file tokens (e.g., separating IDs or synthetic prefixes from target word vowels).

### 2. Formant Extraction & Source-Filter Theory

* **Downsampling**: Audio tracks are dynamically resampled to 16 kHz to bound the Nyquist frequency to 8 kHz, establishing a stable mathematical boundary for Linear Predictive Coding (LPC).
* **Pre-emphasis**: A high-pass filter ( $y(t) = x(t) - \alpha \cdot x(t-1)$ ) is applied to mitigate the natural $-6\text{ dB/octave}$ spectral tilt of human glottal production, accentuating higher resonances like $F_2$.
* **Windowing**: A 25ms central frame is isolated during stable voiced segments and windowed via a Hamming function to minimize spectral leakage caused by rectangular truncation.
* **LPC All-Pole Modeling**: By solving the predictor coefficients and finding the complex roots of the polynomial, vocal tract resonances are converted from polar angles to actual frequencies in Hz to record $F_1$ and $F_2$.

### 3. Acoustic Space Analysis

* Features are projected into an abstract geometric space plotting $F_1$ vs. $-(F_2 - F_1)$.
* This mapping structurally reflects physiological tongue positions (height and advancement), verifying phonetic clusters and detailing structural variances between rigid synthetic models and natural personal physiology.

### 4. Pitch Contour & Time-Frequency Analysis

* The continuous multi-word streams are joined sequentially with 0.5-second structural silent paddings.
* The Probabilistic YIN (`pYIN`) algorithm calculates the fundamental frequency ($F_0$) tracking pitch contours over a log-scaled frequency spectrogram to analyze glottal source behavior across continuous speech frames.

---
