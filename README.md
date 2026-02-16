# NS-3 VANET DDoS Attack Dataset

**NS-3 Simulation Framework for Generating a Novel DDoS Attack Dataset in Vehicular Ad Hoc Network Systems**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![NS-3](https://img.shields.io/badge/NS--3-3.40-blue.svg)](https://www.nsnam.org/)
[![Python](https://img.shields.io/badge/Python-3.8+-green.svg)](https://www.python.org/)

## 📋 Overview

This repository contains a comprehensive NS-3.40-based simulation framework for generating realistic VANET DDoS attack datasets. The framework implements two mobility scenarios (highway and Manhattan grid) with systematic attack variations, enabling machine learning-based intrusion detection research.

### Key Features
- ✅ IEEE 802.11p (WAVE) wireless communication
- ✅ AODV routing protocol
- ✅ Two mobility models: Highway and Manhattan Grid
- ✅ Configurable DDoS attack scenarios (3, 5, 10 attackers)
- ✅ V2V and V2I communication patterns
- ✅ Flash crowd traffic simulation
- ✅ RSU overload and recovery mechanisms
- ✅ NetAnim visualization support
- ✅ CSV packet-level trace logging

## 🎯 Detection Performance

| Model | Highway (3 Att.) | Manhattan (3 Att.) | Highway (10 Att.) | Manhattan (10 Att.) |
|-------|------------------|---------------------|-------------------|---------------------|
| **XGBoost** | **99.40%** | 98.10% | 97.80% | 96.20% |
| **Random Forest** | 99.22% | 97.80% | 97.50% | 95.80% |
| **J48** | 98.85% | 96.90% | 97.00% | 94.90% |
| **KNN** | 98.45% | 96.30% | 96.80% | 94.10% |
| **SVM** | 93.51% | 92.80% | 92.00% | 90.90% |

## 📂 Repository Structure

```
├── simulations/
│   ├── vanet_highway.cc           # Highway mobility simulation
│   ├── vanet_manhattan_ddos.cc    # Manhattan grid simulation
│   └── README.md                  # Compilation instructions
├── datasets/
│   ├── highway.csv
│   └── manhattan.csv
├── images/
│   └── topology_diagrams/         # NetAnim screenshots
├── paper/
│   └── DDoS_VANET_Paper.pdf       # Research paper
└── README.md
```

## 🚀 Quick Start

### Prerequisites
- NS-3.40 or higher
- Python 3.8+
- NetAnim (optional, for visualization)

### 1. Compile NS-3 Simulations

```bash
# Copy simulation files to NS-3 scratch directory
cp simulations/*.cc ~/ns-allinone-3.40/ns-3.40/scratch/

# Build NS-3
cd ~/ns-allinone-3.40/ns-3.40/
./ns3 build

# Run highway simulation (3 attackers)
./ns3 run "vanet_highway --N=40 --runTime=60 --attackStart=20 --attackEnd=40"

# Run Manhattan simulation (5 attackers)
./ns3 run "vanet_manhattan_ddos --N=40 --runTime=60 --attackStart=20 --attackEnd=40"
```

### 2. Process Generated Data

```bash
cd preprocessing/
python flow_aggregation.py --input ../rsu2_received.csv --output flows.csv
python feature_extraction.py --input flows.csv --output features.csv
python smote_balancing.py --input features.csv --output balanced_dataset.csv
```

### 3. Train Machine Learning Models

```bash
cd ml_models/
pip install -r requirements.txt
python train_models.py --dataset ../preprocessing/balanced_dataset.csv --output results/
```

## 📊 Dataset Specifications

### Raw Packet Attributes
- Event ID
- Timestamp (seconds)
- Source/Destination IP
- Source/Destination Port
- Packet Type (normal/attack)
- Packet Length (bytes)

### Extracted Flow Features
1. **Packet Size Distribution** - Mean, variance, min, max
2. **Inter-Arrival Time Statistics** - Mean, std deviation
3. **Port Usage Patterns** - V2I (9001→9002), V2V (9005→9004), Attack (9003→9002)
4. **Traffic Burst Characteristics** - Packets per second
5. **Flow Duration** - Total session length
6. **RSU Load Rate** - Aggregate PPS at infrastructure

### Attack Scenarios
- **3 Attackers**: 450 PPS aggregate (baseline)
- **5 Attackers**: 750 PPS aggregate (moderate)
- **10 Attackers**: 1500 PPS aggregate (severe)


## 👥 Contributors

- **Martina Morcos Halim** - *Primary Author* - [martinamorcos29](https://github.com/martinamorcos29)
- **Prof. Maha El Sabrouty** - *Supervisor*

## 🙏 Acknowledgments

- Egypt Japan University of Science and Technology
- NS-3 Development Team
- VANET Security Research Community

## 📧 Contact

For questions or collaborations:
- GitHub Issues: [Create an issue](https://github.com/martinamorcos29/NS3-VANET-DDoS-Attack/issues)

---

**Note**: This dataset is provided for research purposes only. Simulated attack patterns should not be used for malicious activities.
