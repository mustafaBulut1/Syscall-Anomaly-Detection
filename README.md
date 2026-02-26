# LSTM-Based Real-Time Syscall Anomaly Detection (HIDS)

This project implements a **Host-based Intrusion Detection System (HIDS)** using Deep Learning (LSTM) to detect malicious activities by analyzing Linux System Call (Syscall) sequences. Achieving an **Accuracy of 95.90%** and an **F1-Score of 0.9574**, the model demonstrates high reliability in distinguishing between normal system operations and cyberattacks.



## 🛠 What Did I Do?
I developed an end-to-end deep learning pipeline that processes raw system call data into actionable security intelligence.

* **Custom Data Engineering**: Built a robust `SyscallDataset` class capable of handling the ADFA-LD dataset, featuring manual stratified splitting to ensure balanced class distribution (833 Normal vs 746 Attack samples).
* **Neural Architecture**: Designed a multi-layer LSTM model with **Masked Mean Pooling** to process variable-length system call sequences while ignoring padding noise.
* **Optimization**: Integrated advanced training techniques including **ID Shifting** (to reserve 0 for padding), **Class Weighting**, and **SafeTensors** for secure and fast model serialization.

## 🧠 How Did I Do It? (Technical Deep Dive)
The project addresses several common pitfalls in AI-based security:

1.  **Solving Padding Dilution**: Standard LSTMs often lose signal in long sequences due to padding. I implemented a **Masked Mean Pooling** layer that calculates the average of only non-zero (actual syscall) embeddings, ensuring the signal remains strong regardless of sequence length.
2.  **ID Shifting Strategy**: Since the `read()` syscall often has the ID `0`, I shifted all syscall IDs by `+1`. This allows the model's `Embedding` layer to treat `0` strictly as a mathematical padding token without "clashing" with legitimate system calls.
3.  **Data Integrity**: Used a **Stratified Split** (80% Train, 20% Test) instead of a simple random split. This prevents "Data Leakage" where identical attack signatures might appear in both sets, ensuring the **97.33% Recall** is a genuine reflection of the model's generalization capability.

## 🚀 Why Does It Matter to the Company?
In a modern enterprise environment, traditional signature-based antiviruses fail against **Zero-Day attacks**. This project provides:

* **Behavioral Defense**: Unlike static signatures, this model learns the *intent* and *pattern* of a process, allowing it to detect never-before-seen malicious behaviors based on system interaction.
* **Real-Time Potential**: With a sequence limit of 500 and efficient LSTM layers, this model is designed for low-latency inference, making it suitable for integration into **eBPF-based** real-time kernel monitoring agents.
* **Operational Reliability**: High **Precision (94.19%)** means fewer false alarms. For a Security Operations Center (SOC), this directly translates to reduced "alert fatigue" and faster response times to actual threats.

## 📊 Performance Metrics
The final evaluation on unseen data yielded the following results:

| Metric | Result | Impact |
| :--- | :--- | :--- |
| **Accuracy** | **95.90%** | Overall correctness of system classifications. |
| **Precision** | **94.19%** | High trust in alerts; minimal false positives. |
| **Recall** | **97.33%** | Superior capability in catching actual attacks. |
| **F1-Score** | **0.9574** | Mathematical balance between Precision and Recall. |

## 📂 Project Structure
```text
├── data/               # ADFA-LD Dataset (Normal & Attack Masters)
├── models/             # Saved .safetensors and .pth models
├── src/
│   └── dataset.ipynb   # Full training and evaluation pipeline
└── README.md
