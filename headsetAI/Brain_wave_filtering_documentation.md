edicational videos on how this works
https://www.youtube.com/watch?v=tGs2BrxVxUQ&list=PLo_IgZ-NTRcgWG5fm8BCs1skt3CIBtLx1
https://www.youtube.com/watch?v=KrJaJDMBo54&list=PLo_IgZ-NTRcgWG5fm8BCs1skt3CIBtLx1&index=2

EXAMPLE CODE                                                                                                                                                                                                                                                                                  import numpy as np
from scipy.signal import butter, filtfilt
import matplotlib.pyplot as plt

def butter_bandpass(lowcut, highcut, fs, order=4):
    nyquist = 0.5 * fs
    low = lowcut / nyquist
    high = highcut / nyquist
    # Design Butterworth filter
    b, a = butter(order, [low, high], btype='band')
    return b, a

def bandpass_filter(data, lowcut, highcut, fs, order=4):
    b, a = butter_bandpass(lowcut, highcut, fs, order=order)
    # Apply zero-phase filter
    y = filtfilt(b, a, data)
    return y

#Simulated EEG data
fs = 256  # Sampling frequency in Hz
t = np.linspace(0, 1.0, fs)
raw_eeg_data = np.sin(2 * np.pi * 10 * t)  # 10 Hz sine wave (alpha band)

#Add noise
noise = 0.5 * np.random.randn(len(t))
raw_eeg_data += noise

#Filter parameters
lowcut = 8.0   # Lower frequency for alpha band
highcut = 12.0  # Upper frequency for alpha band

#Apply band-pass filter
filtered_data = bandpass_filter(raw_eeg_data, lowcut, highcut, fs, order=4)

#Plotting
plt.figure(figsize=(12, 6))
plt.subplot(2,1,1)
plt.plot(t, raw_eeg_data)
plt.title('Raw EEG Data with Noise')
plt.subplot(2,1,2)
plt.plot(t, filtered_data, color='orange')
plt.title('Filtered EEG Data (Alpha Band)')
plt.tight_layout()
plt.show()

OUTPUT OF CODE
![image](https://github.com/user-attachments/assets/44dc09ea-9e35-4e1a-865c-21d54638754a)


in length documentation on EEG Filtration generated by gpt o1

Title: Advanced Filtering Techniques in EEG Data Analysis

Introduction

Electroencephalography (EEG) is a non-invasive method to record electrical activity of the brain. Due to the complex and noisy nature of EEG signals, filtering is an essential preprocessing step in EEG data analysis. This lecture will delve into the intricacies of EEG data filtering, exploring the types of noise encountered, the mathematical foundations of various filters, practical implementation strategies, and considerations for effective filtering.

Importance of Filtering in EEG Data
1.1. Nature of EEG Signals

Complexity: EEG signals are the result of the synchronous activity of thousands or millions of neurons. They are characterized by low amplitude (typically 10–100 μV) and a wide frequency range (0.1–100 Hz).
Sensitivity to Noise: EEG recordings are highly susceptible to various forms of noise and artifacts due to the small signal amplitudes.
1.2. Sources of Noise in EEG Data

Power Line Interference:
Frequency: 50 Hz (Europe, Asia) or 60 Hz (North America).
Cause: Electromagnetic fields from electrical systems and devices.
Electromyographic (EMG) Artifacts:
Muscle Activity: Facial movements, jaw clenching, and neck tension can introduce high-frequency noise (>30 Hz).
Electrooculographic (EOG) Artifacts:
Eye Movements and Blinks: Generate large amplitude potentials that can overshadow EEG signals, primarily in the delta (0.5–4 Hz) and theta (4–8 Hz) bands.
Baseline Drift:
Low-Frequency Noise: Caused by perspiration, slow movements, or electrode impedance fluctuations, affecting frequencies below ~0.5 Hz.
Electrode and Cable Movements:
Mechanical Artifacts: Movement of electrodes or cables can introduce transient artifacts across various frequencies.
1.3. Impact on Data Analysis
NEW
[1:52 PM]
Signal Distortion: Noise can mask or mimic neural activity, leading to inaccurate interpretations.
Frequency Overlaps: Some artifacts overlap with the frequency bands of interest (e.g., muscle artifacts in the beta band), complicating the separation of noise from genuine EEG signals.
Fundamentals of Signal Filtering
2.1. Concept of Filtering

Definition: Filtering is the process of modifying or removing parts of a signal within certain frequency ranges.
Purpose in EEG: Enhance the signal-to-noise ratio (SNR) by attenuating unwanted frequency components while preserving the neural signals of interest.
2.2. Types of Filters

a. Low-Pass Filters (LPF)

Function: Allow frequencies below a specified cutoff frequency fc to pass while attenuating higher frequencies,

Mathematical Representation:

![image](https://github.com/user-attachments/assets/5f7c845c-442d-4229-bd1a-0fd509f0a9c3)

Applications in EEG:
Removing High-Frequency Noise: Attenuate EMG artifacts and electrical interference above the EEG frequency range.
Preserving Slow Waves: Ensure delta and theta activities are retained for sleep studies or deep brain activity analysis.
b. High-Pass Filters (HPF)

Function: Allow frequencies above a specified cutoff frequency 𝑓𝑐
  to pass while attenuating lower frequencies.
Mathematical Representation:

![image](https://github.com/user-attachments/assets/207011e7-f412-4932-bb44-a1707cfc7d12)

Applications in EEG:
Removing Baseline Drift: Eliminate slow fluctuations and DC offsets.
Improving Epoch Alignment: Enhance the temporal precision of event-related potentials (ERPs).
c. Band-Pass Filters (BPF)

Function: Allow frequencies within a specified range 
[𝑓𝑙𝑜𝑤,𝑓ℎ𝑖𝑔ℎ]
[f low,f high] to pass while attenuating frequencies outside this range.
Mathematical Representation:

![image](https://github.com/user-attachments/assets/37616040-fcfd-4809-b7c9-7815541dcbe4)

Applications in EEG:
Isolating Specific Brainwaves: Focus on alpha (8–12 Hz), beta (13–30 Hz), or gamma (>30 Hz) bands for cognitive studies.
Enhancing SNR in Target Bands: Suppress noise outside the band of interest.
d. Notch Filters (Band-Stop Filters)

Function: Attenuate a narrow frequency band around a center frequency 𝑓0
  while allowing other frequencies to pass.
Mathematical Representation:

![image](https://github.com/user-attachments/assets/90d6b81b-5554-4201-ada2-dbfe2f2ba334)

Applications in EEG:
Eliminating Power Line Noise: Suppress 50/60 Hz interference without affecting adjacent frequencies.
Removing Specific Artifacts: Target narrow-band interference from equipment.
Mathematical Foundations of Filters
3.1. Filter Design Basics

Linear Time-Invariant (LTI) Systems: Filters are often modeled as LTI systems characterized by their impulse response 
ℎ(𝑡) h(t) or frequency response 𝐻(𝑓)
H(f).
Convolution Operation:

![image](https://github.com/user-attachments/assets/1e9050a2-d9fe-4c61-be78-fc2ca8e86835)
![image](https://github.com/user-attachments/assets/b85c2029-5299-451f-ba7f-f630621064c8)
![image](https://github.com/user-attachments/assets/77b4e222-ecf3-4338-8e69-5583a1bbf893)

3.3. Common Filter Design Methods

Butterworth Filters:
Characteristics: Maximally flat frequency response in the passband.
Usage: Smooth roll-off without ripples; good for general-purpose filtering.
Chebyshev Filters:
Characteristics: Allow ripples in the passband (Type I) or stopband (Type II) to achieve a steeper roll-off.
Elliptic Filters:
Characteristics: Ripples in both passband and stopband; steepest roll-off for a given filter order.
Bessel Filters:
Characteristics: Linear phase response in the passband; preserves waveform shape.
3.3. Common Filter Design Methods

Butterworth Filters:
Characteristics: Maximally flat frequency response in the passband.
Usage: Smooth roll-off without ripples; good for general-purpose filtering.
Chebyshev Filters:
Characteristics: Allow ripples in the passband (Type I) or stopband (Type II) to achieve a steeper roll-off.
Elliptic Filters:
Characteristics: Ripples in both passband and stopband; steepest roll-off for a given filter order.
Bessel Filters:
Characteristics: Linear phase response in the passband; preserves waveform shape.

![image](https://github.com/user-attachments/assets/32b1fa2c-5870-4bc5-a680-5a1472622542)

from scipy.signal import butter, filtfilt

def butter_bandpass(lowcut, highcut, fs, order=4):
    nyquist = 0.5 * fs
    low = lowcut / nyquist
    high = highcut / nyquist
    # Design Butterworth filter
    b, a = butter(order, [low, high], btype='band')
    return b, a

def bandpass_filter(data, lowcut, highcut, fs, order=4):
    b, a = butter_bandpass(lowcut, highcut, fs, order=order)
    # Apply zero-phase filter
    y = filtfilt(b, a, data)
    return y

b. Zero-Phase Filtering with filtfilt

Purpose: Eliminates phase distortion by filtering the data in both forward and backward directions.
Benefit: Preserves the original waveform shape, which is crucial for event-related analyses.

4.3. Example Usage

import numpy as np
import matplotlib.pyplot as plt

Simulated EEG data
fs = 256  # Sampling frequency in Hz
t = np.linspace(0, 1.0, fs)
raw_eeg_data = np.sin(2 * np.pi * 10 * t)  # 10 Hz sine wave (alpha band)

Add noise
noise = 0.5 * np.random.randn(len(t))
raw_eeg_data += noise

Filter parameters
lowcut = 8.0   # Lower frequency for alpha band
highcut = 12.0  # Upper frequency for alpha band

Apply band-pass filter
filtered_data = bandpass_filter(raw_eeg_data, lowcut, highcut, fs, order=4)

Plotting
plt.figure(figsize=(12, 6))
plt.subplot(2,1,1)
plt.plot(t, raw_eeg_data)
plt.title('Raw EEG Data with Noise')
plt.subplot(2,1,2)
plt.plot(t, filtered_data, color='orange')
plt.title('Filtered EEG Data (Alpha Band)')
plt.tight_layout()
plt.show()

Choosing Filter Parameters
5.1. Sampling Frequency (𝑓𝑠)

Nyquist Criterion: To accurately capture frequencies up to 
𝑓𝑚𝑎𝑥
 , the sampling rate must be at least 2𝑓𝑚𝑎𝑥​.
Typical EEG Sampling Rates: 256 Hz, 512 Hz, or higher for high-frequency analysis.
5.2. Cutoff Frequencies
High-Pass Filter Cutoff (𝑓𝐻𝑃 ​)

Typical Values: 0.1–1 Hz.
Considerations: A higher cutoff may remove slow cortical potentials and important low-frequency components.
Low-Pass Filter Cutoff (𝑓𝐿𝑃​)

Typical Values: 30–70 Hz.
Considerations: Should be set below the Nyquist frequency and above the highest frequency of interest.
5.3. Filter Order

Definition: The order of the filter determines the steepness of the transition band.
Trade-Offs:
Higher Order:
Pros: Sharper cutoff, better attenuation of unwanted frequencies.
Cons: Increased computational load, potential for instability (IIR filters), more phase distortion.
Common Practice: Use an order between 2 and 8, balancing performance and efficiency.

Common Artifacts and Limitations of Filtering
6.1. Phase Distortion

Definition: A change in the phase relationship between different frequency components of the signal.
Impact on EEG:
Latency Shifts: Can distort the timing of EEG events, affecting ERP analyses.
Waveform Alteration: Non-linear phase filters can distort the shape of EEG waveforms.
Mitigation Strategies:

Zero-Phase Filtering: Use filtfilt function to apply the filter forward and backward.
Linear Phase Filters: Design filters with linear phase characteristics (e.g., certain FIR filters).
6.2. Edge Effects

Definition: Artifacts introduced at the beginning and end of the filtered signal due to the filter's impulse response.
Cause: Filters require data points beyond the signal edges to fully respond, which are not available.
Mitigation Strategies:

Signal Padding: Extend the signal by mirroring or repeating data to provide additional points for filtering.
Discarding Edge Samples: Remove a portion of data at the edges post-filtering.
6.3. Filter Ringing

Definition: Oscillations near sharp transitions in the signal or at the cutoff frequencies.
Cause: Sharp cutoffs and high filter orders can introduce Gibbs phenomenon.
Mitigation Strategies:

Lower Filter Order: Use smoother filters to reduce ringing.
Windowing Techniques: Apply window functions in FIR filter design to reduce sidelobes.
6.4. Loss of Important Data

Risk: Overly aggressive filtering can remove or attenuate genuine EEG components.
Examples:
High High-Pass Cutoff: Removing slow waves (delta band) critical in sleep studies.
Low Low-Pass Cutoff: Eliminating gamma band activity relevant in certain cognitive tasks.
Mitigation Strategies:

Careful Selection of Cutoffs: Base cutoff frequencies on the specific EEG components of interest.
Data-Driven Decisions: Analyze the frequency content of your data before deciding on filter parameters.

Advanced Filtering Techniques
7.1. Adaptive Filtering

Purpose: Adjusts filter characteristics in real-time based on the signal properties.
Applications:
Removing Non-Stationary Noise: Effective against artifacts that change over time.
Techniques: Least Mean Squares (LMS) algorithms, Recursive Least Squares (RLS).
7.2. Independent Component Analysis (ICA)

Purpose: Decomposes the EEG signals into statistically independent components.
Applications:
Artifact Removal: Identify and remove components corresponding to artifacts (e.g., eye blinks).
Process:
Decomposition: Apply ICA to the preprocessed EEG data.
Component Identification: Use topographies and time courses to identify artifact-related components.
Reconstruction: Reconstruct the EEG data without the identified components.
7.3. Wavelet Filtering

Purpose: Analyze signals with time-varying frequency content.
Advantages:
Time-Frequency Localization: Better suited for non-stationary signals like EEG.
Applications:
Artifact Reduction: Suppress artifacts in specific time-frequency regions.
Techniques:
Discrete Wavelet Transform (DWT): Decompose signal into wavelet coefficients.
Thresholding: Modify coefficients associated with noise/artifacts.
Reconstruction: Reconstruct the signal from the modified coefficients.
[2:00 PM]
Practical Guidelines for EEG Filtering
8.1. Know Your Data

Signal Characteristics: Understand the frequency content and temporal dynamics of your EEG signals.
Artifact Profiles: Be aware of the types and characteristics of artifacts present.
8.2. Balance Between Signal Preservation and Noise Reduction

Minimal Filtering Principle: Apply the least amount of filtering necessary to achieve your analysis goals.
Iterative Approach: Start with conservative filter settings and adjust as needed based on the results.
8.3. Documentation and Reporting

Transparency: Document filter types, parameters, and the rationale behind their selection.
Reproducibility: Ensure that others can replicate your preprocessing steps.
8.4. Validation

Visual Inspection: Examine raw and filtered signals to assess the impact of filtering.
Quantitative Measures: Use metrics like SNR, power spectral density, and artifact indices to evaluate filtering effectiveness.

Conclusion
Filtering is a critical step in EEG data analysis, enabling researchers to enhance the quality of their recordings and focus on neural signals of interest. By understanding the mathematical foundations of filters, the nature of EEG artifacts, and the practical considerations in filter implementation, you can effectively preprocess EEG data for accurate and reliable analysis. Always consider the balance between noise reduction and signal preservation, and remain mindful of the potential limitations and artifacts introduced by filtering.

Further Reading and Resources

Books:

"EEG Signal Processing" by Saeid Sanei and J. A. Chambers.
"Biomedical Signal Processing and Signal Modeling" by Eugene N. Bruce.
Articles:

Widmann, A., Schröger, E., & Maess, B. (2015). Digital filter design for electrophysiological data – a practical approach. Journal of Neuroscience Methods, 250, 34-46.
Acunzo, D. J., MacKenzie, G., & van Rossum, M. C. (2012). Systematic biases in early ERP and ERF components as a result of high-pass filtering. Journal of Neuroscience Methods, 209(1), 212-218.
Online Resources:

MNE-Python Tutorials: https://mne.tools/stable/auto_tutorials/index.html
EEGLAB Tutorial: https://eeglab.org/tutorials.html




