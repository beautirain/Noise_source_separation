🔊 Substation Transformer Noise Separation & Evaluation

Full dataset and evaluation details will be released after paper acceptance

A deep learning–based framework for transformer noise extraction and acoustic evaluation under complex multi-source noise environments
📌 Project Overview
This repository presents a target-oriented noise source separation framework designed for substation transformer noise analysis and intelligent acoustic evaluation.

Unlike generic speech separation tasks, this project focuses on extracting steady-state transformer equipment noise from complex acoustic environments (urban noise, wind, rain, human activities, etc.), and quantitatively evaluating the separation quality using engineering-relevant acoustic metrics.
Key features:

🎯 Target source extraction (Transformer vs. All Other Noises)

🔊 Time-domain deep separation models (ConvTasNet / DPRNN / DPTNet)

📊 Comprehensive evaluation metrics:

    SI-SDR improvement

    A-weighted SPL recovery error

    Spectral fidelity (SC, HEF)

🌐 Robustness testing under multi-source interference

📈 Visualization tools for spectra, spectrograms, and training curves
Core Idea
The project adopts a binary source separation strategy:


  Mixed Noise  
  
     ├── Transformer Noise (Target)
     └─Non-Transformer Noise (All Interferences)
This formulation enables robust extraction of transformer noise even when multiple environmental noise sources coexist, which is more suitable for substation acoustic assessment than fine-grained multi-class separation.
   
   Project Structure
   .
   
    ├── checkpoints/                  # Trained model checkpoints
    ├── data_raw/                     # Raw long-duration recordings
    ├── data_clean_segments/          # Segmented clean transformer signals
    ├── data_mixtures/                # Two-source (transformer + noise) datasets
    ├── data_multisource/             # Multi-source interference datasets
    ├── noise_dir/                    # Environmental / urban noise library
    ├── target_dir/                   # Transformer noise library
    ├── separation_results/           # Separated audio outputs
    ├── eval_results/                 # Quantitative evaluation outputs
    │
    ├── train_asteroid.py             # Model training script
    ├── run_separation.py             # Inference & separation
    │
    ├── make_multisource_dataset.py   # Multi-source dataset generator
    ├── create_mixtures.py            # Two-source mixture generation
    ├── prepare_clean_segments.py     # Long audio segmentation
    │
    ├── eval_multisource_target.py    # Target extraction evaluation (core)
    ├── eval_spl_error.py             # A-weighted SPL error evaluation
    ├── eval_spectrum_metrics.py      # Spectral fidelity metrics
    │
    ├── plot_loss_curves.py            # Training curves visualization
    ├── plot_model_compare.py         # Model performance comparison
    ├── plot_spectrogram_heatmap.py   # Spectrogram heatmaps
    ├── plot_spectrum_compare.py      # Spectrum comparison
    ├── plot_spectrum_bar.py           # Spectrum metric bar charts
    │
    └── README.md

Supported Models
Implemented via Asteroid framework:
  | Model      | Description                    | Characteristics                               |
  | ---------- | ------------------------------ | --------------------------------------------- |
  | ConvTasNet | Temporal convolutional network | Stable, efficient, best overall balance       |
  | DPRNN      | Dual-path RNN                  | Strong sequence modeling, risk of overfitting |
  | DPTNet     | Transformer-based              | High capacity, optimization sensitive         |

All models are trained as 2-output separators (target vs. interference).

📊 Evaluation Metrics

1️⃣ SI-SDR Improvement (SI-SDRi)

  Measures source separation quality relative to the input mixture:
  
  SI-SDR𝑖=SI-SDRest−SI-SDRmix
  
2️⃣ A-weighted Sound Pressure Level (SPL) Error

  Evaluates engineering accuracy of extracted transformer noise:
  
  Δ𝐿𝐴=𝐿𝐴,est−𝐿𝐴,true
  
  Reported as:
  Mean / max absolute error
    
  Percentage of samples with ∣Δ𝐿𝐴∣<1 dB
    
3️⃣ Spectral Fidelity Metrics

  SC (Spectral Correlation)
  
  Measures similarity of spectral shape

    
  HEF (Harmonic Energy Fidelity)
  
  Quantifies recovery of harmonic energy at fundamental and multiples
    
🧪 Multi-Source Robustness Evaluation

To assess real-world robustness, multi-source mixtures are generated:

  Transformer + Noise_1 + Noise_2 + ... + Noise_K
  
The model still outputs 2 channels, and the target transformer channel is automatically identified using:
  Spectral correlation–based channel selection
  
  (Optional) SI-SDR oracle selection for upper-bound analysis
Quick Start

1️⃣ Train a model

    python train_asteroid.py --model_type convtasnet --mode train
    
2️⃣ Generate multi-source dataset

    python make_multisource_dataset.py \
      --target_dir target_dir \
      --noise_dir noise_dir \
      --out_root data_multisource \
      --seg_sec 4 \
      --train 6000 --val 600 --test 600 \
      --kmin 3 --kmax 6
      
3️⃣ Evaluate target extraction (recommended)

    python eval_multisource_target.py \
      --data_root data_multisource \
      --model_type convtasnet \
      --ckpt checkpoints/convtasnet_epoch30.pth \
      --pick_method spec \
      --f0 100
      
📈 Visualization

Training convergence curves

Spectrogram heatmaps (true vs separated vs mixed)

Spectrum comparison plots

Metric bar charts across models

All plotting scripts are under plot_*.py.

🧩 Design Philosophy

✔ Focus on engineering relevance, not only signal-level metrics

✔ Target-oriented extraction instead of over-complicated multi-class separation


✔ Modular and reproducible experimental pipeline

📚 Potential Extensions

Multi-target extraction (transformer + reactor + corona)

Online real-time deployment on NI PXIe platforms

Integration with fuzzy logic–based acoustic impact assessment

Joint separation + classification architecture

📜 License

This project is intended for academic research and engineering applications.

Please cite or acknowledge if used in publications.
🙌 Acknowledgements

Asteroid for separation framework

Open environmental noise datasets
