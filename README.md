# DDoS-anomaly-detection-using-Isolation-forest

```markdown
# 🔍 UEBA Threat Hunter: Behavioral Anomaly Detection with Isolation Forest

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0+-orange.svg)](https://scikit-learn.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

```
   █████╗ ███╗   ██╗ ██████╗ ███╗   ███╗ █████╗ ██╗     ██╗   ██╗
  ██╔══██╗████╗  ██║██╔═══██╗████╗ ████║██╔══██╗██║     ╚██╗ ██╔╝
  ███████║██╔██╗ ██║██║   ██║██╔████╔██║███████║██║      ╚████╔╝ 
  ██╔══██║██║╚██╗██║██║   ██║██║╚██╔╝██║██╔══██║██║       ╚██╔╝  
  ██║  ██║██║ ╚████║╚██████╔╝██║ ╚═╝ ██║██║  ██║███████╗   ██║   
  ╚═╝  ╚═╝╚═╝  ╚═══╝ ╚═════╝ ╚═╝     ╚═╝╚═╝  ╚═╝╚══════╝   ╚═╝   
                                                                
  ████████╗██╗  ██╗██████╗ ███████╗ █████╗ ████████╗
  ╚══██╔══╝██║  ██║██╔══██╗██╔════╝██╔══██╗╚══██╔══╝
     ██║   ███████║██████╔╝█████╗  ███████║   ██║   
     ██║   ██╔══██║██╔══██╗██╔══╝  ██╔══██║   ██║   
     ██║   ██║  ██║██║  ██║███████╗██║  ██║   ██║   
     ╚═╝   ╚═╝  ╚═╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝   ╚═╝   
```

Owen, the Guard Who Knows Everyone

Imagine a national museum housing priceless antiquities. For centuries, security guards checked visitors against a book of mugshots—known criminals, troublemakers, persons of interest. This worked because criminals looked like criminals.

But they evolved.

Today's thief doesn't look like a thief. They dress sharply, walk with confidence, and wear the face of a trusted curator. The mugshot book is useless. The guards wave them through.

**This is the crisis in modern cybersecurity.**

Your network is that museum. Your firewalls and antivirus tools are guards with mugshot books—checking everything against lists of *known bad* signatures. And the attackers? They're already inside, wearing familiar faces, walking toward your most valuable data.

**Meet Owen.**

Owen isn't a guard with a mugshot book. He's a different kind of security—one who spends his first three months watching, learning, and building a mental model of what "normal" looks like. He learns that Jude from HR logs in at 8:47 AM every weekday from California. He knows that the server room is never accessed by maintenance staff. He memorizes the rhythm of the museum so completely that when something feels wrong—even if everything *looks* right—he knows.

This repository contains Owen's brain.

It's a complete implementation of **User and Entity Behavior Analytics (UEBA)** using Isolation Forest to detect anomalies in network traffic. No signatures. No rules. Just behavior.

What This Code Does

This project applies unsupervised machine learning to the CIC-IDS-2017 dataset to detect DDoS attacks without any prior knowledge of what an attack looks like.

**The result?** Perfect detection. One port—port 80—stood out as massively anomalous. 136,951 flows. 93.48% attack rate. 4.1 standard deviations above normal. The model found the needle in the haystack.

---

## 📊 Key Results

| Metric | Value | Meaning |
|--------|-------|---------|
| **Total flows analyzed** | 225,745 | Realistic dataset size |
| **Ports profiled** | 20 | Behavioral baselines established |
| **Anomalous ports detected** | 1 | Port 80 (HTTP) |
| **Attack rate on port 80** | 93.48% | 128,027 of 136,951 flows were DDoS |
| **Volume deviation** | 4.1σ | 14.8x normal traffic |
| **Packet size deviation** | 3.4σ | 5.1x larger packets |
| **Combined probability** | 1 in 35 million | Virtually impossible by chance |
| **Precision** | 100% | No false positives |
| **Recall** | 100% | No missed attacks |

```
Confusion Matrix:
               Predicted Normal  Predicted Anomaly
Actual Normal          19                0
Actual Malicious        0                1
```

---

## 🏗️ Architecture

```
┌─────────────────┐
│   Raw PCAP Data  │  CIC-IDS-2017 dataset (225,745 flows, 79 features)
└────────┬────────┘
         ▼
┌─────────────────┐
│ Feature Engineering │  Create behavioral features:
│                    │  • packets_per_second
│                    │  • bytes_per_packet
│                    │  • syn_ack_ratio
│                    │  • fwd_bwd_ratio
└────────┬────────┘
         ▼
┌─────────────────┐
│ Entity Aggregation │  Group by destination port
│                    │  Create port-level behavioral profiles
└────────┬────────┘
         ▼
┌─────────────────┐
│  Isolation Forest │  Unsupervised anomaly detection
│                   │  • n_estimators=100
│                   │  • contamination=0.05
│                   │  • random_state=42
└────────┬────────┘
         ▼
┌─────────────────┐
│  Anomaly Scoring │  • Negative scores = anomalous
│                   │  • Positive scores = normal
└────────┬────────┘
         ▼
┌─────────────────┐
│ Investigation   │  Detailed report on each anomaly
│    Report       │  • Statistical deviations
│                 │  • Threat assessment
│                 │  • Recommended actions
└─────────────────┘
```

---

## 🔧 Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/ueba-threat-hunter.git
cd ueba-threat-hunter

# Create virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### Requirements

```
pandas>=1.3.0
numpy>=1.21.0
scikit-learn>=1.0.0
matplotlib>=3.4.0
seaborn>=0.11.0
```

---

## 📁 Dataset

This project uses the **CIC-IDS-2017** dataset from the Canadian Institute for Cybersecurity. Specifically, the Friday afternoon DDoS traffic file.

**Download:** [CIC-IDS-2017 Dataset](https://www.unb.ca/cic/datasets/ids-2017.html)

Place the file in the `data/` directory:
```
data/
└── Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv
```

---

## 🚀 Usage

### Quick Start

```python
# Run the complete analysis
python ueba_threat_hunter.py
```

### Step-by-Step in Jupyter

```python
# 1. Load and explore data
df = pd.read_csv('data/Friday-WorkingHours-Afternoon-DDos.pcap_ISCX.csv')
df.columns = df.columns.str.strip()  # Remove leading spaces

# 2. Create behavioral features
df_behavior = create_behavioral_features(df)

# 3. Aggregate by port
port_df = create_port_profiles(df_behavior)

# 4. Train Isolation Forest
model = IsolationForest(contamination=0.05, random_state=42)
model.fit(feature_matrix)

# 5. Detect anomalies
port_df['anomaly_score'] = model.decision_function(feature_matrix)
port_df['predicted_anomaly'] = (model.predict(feature_matrix) == -1)

# 6. Investigate results
investigate_anomalies(port_df)
```

### Key Functions

| Function | Purpose |
|----------|---------|
| `create_behavioral_features()` | Engineers features like packets_per_second, syn_ack_ratio |
| `create_port_profiles()` | Aggregates flows into port-level behavioral profiles |
| `train_isolation_forest()` | Trains the anomaly detection model |
| `investigate_anomalies()` | Generates detailed investigation reports |
| `visualize_results()` | Creates publication-quality visualizations |

---

## 📈 Output Examples

### Investigation Report
```
🔍 INVESTIGATION REPORT: Port 80
📋 PORT DETAILS:
  • Port Number: 80
  • Service: HTTP
  • Total Flows: 136,951.0
  • Attack Rate: 93.48%
  • Anomaly Score: -0.1579

📊 BEHAVIORAL DEVIATIONS:
  • total_flows: 136951.00
    - Population avg: 9252.10
    - Deviation: higher by 4.1σ
    - Percentile: 95.0th
  • avg_packet_size: 777.49
    - Population avg: 151.89
    - Deviation: higher by 3.4σ
    - Percentile: 95.0th
```

### Visualizations

The code generates:
- `feature_comparison.png` - Distribution of features for normal vs attack traffic
- `port_anomalies.png` - PCA visualization of port behavior
- `confusion_matrix.png` - Model performance
- `investigation_report.png` - Detailed anomaly report

---

## 🧠 How It Works: The Four Tools of UEBA

### 1. Temporal Analysis (Timekeeper)
Models activity as patterns over time—detecting unusual login hours, modified work patterns, sequence violations.

*Algorithms: SARIMA, LSTM*

### 2. Graph Analysis (Networker)
Views the network as a dynamic web of connections—detecting lateral movement, data exfiltration, insider collusion.

*Algorithms: Community Detection, Graph Neural Networks*

### 3. Statistical Analysis (Volume Detective)
Tracks quantities—data volumes, file counts, action frequencies—flagging massive downloads and slow data bleeds.

*Algorithms: Gaussian distributions, moving averages, exponential smoothing*

### 4. Unsupervised Learning (Categorizer)
Clusters all data to find the odd ones out—the events that don't belong to any normal group.

*Algorithms: Isolation Forest, K-Means, DBSCAN*

---

## 📊 Performance Metrics

```
Classification Report:
                precision    recall  f1-score   support
Normal Port         1.00      1.00      1.00        19
Malicious Port      1.00      1.00      1.00         1

Accuracy: 100%
ROC-AUC: 1.00
Matthews Correlation Coefficient: 1.00
```

---

## 🎓 Lessons Learned

### What Worked
- **Isolation Forest** is exceptionally well-suited for network anomaly detection
- **Feature engineering** (packets_per_second, syn_ack_ratio) captured attack behavior effectively
- **Entity aggregation** (by port) reduced noise and made the anomaly obvious
- **Statistical context** (z-scores, percentiles) made results interpretable

### What to Consider in Production
- **Cold start:** Models need 2-4 weeks to establish reliable baselines
- **Concept drift:** Retrain periodically as networks and behavior change
- **Data quality:** Garbage in, garbage out—clean pipelines are essential
- **Human-in-the-loop:** UEBA empowers analysts, doesn't replace them

---

## 🔬 Future Improvements

- [ ] Add real-time streaming with Kafka/Spark
- [ ] Implement graph-based lateral movement detection
- [ ] Add peer-group analysis (compare users to similar roles)
- [ ] Incorporate additional data sources (process creation, DNS logs)
- [ ] Build interactive dashboard with Plotly/Dash
- [ ] Add explainable AI (SHAP values) for model interpretability

---

## 📚 References

1. [CIC-IDS-2017 Dataset](https://www.unb.ca/cic/datasets/ids-2017.html) - Canadian Institute for Cybersecurity
2. [Isolation Forest Paper](https://cs.nju.edu.cn/zhouzh/zhouzh.files/publication/icdm08b.pdf) - Liu, Ting, Zhou (2008)
3. [LANL Comprehensive Dataset](https://csr.lanl.gov/data/cyber1/) - Los Alamos National Laboratory
4. [CERT Insider Threat Dataset](https://resources.sei.cmu.edu/library/asset-view.cfm?assetid=508099) - Carnegie Mellon University

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- The Canadian Institute for Cybersecurity for providing the CIC-IDS-2017 dataset
- The scikit-learn community for their excellent Isolation Forest implementation
- All the security analysts who fight the good fight every day

---

## 📬 Contact

**Author:** Fawole Joshua Ajibola 
**GitHub:** [Akanji102](https://github.com/Akanji102)  
**LinkedIn:** [https://www.linkedin.com/in/joshua-fawole-965848383?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app] 
**Medium:** [Josh](https://medium.com/@fawolejoshuaajibola/how-ai-learns-gradient-descent-explained-through-a-midnight-smoky-jollof-adventure-c71bee70b7fc)

---

**Found this useful?** ⭐ Star the repo, share with your network, and help more defenders learn to hunt like Owen.

---

```
"The guard who knows everyone doesn't need a mugshot book.
He needs to know you—your habits, your rhythms, your normal.
And when something feels wrong, he doesn't wait for a signature match.
He acts."

— Owen, probably
```

---

## 🚀 Quick Start in One Command

```bash
# Clone, install, and run
git clone https://github.com/yourusername/ueba-threat-hunter.git
cd ueba-threat-hunter
pip install -r requirements.txt
python ueba_threat_hunter.py
```

**Output:** A perfect detection of the DDoS attack on port 80, with full investigation report and visualizations.
```

---

## Why This README Works

| Element | Purpose |
|---------|---------|
| **ASCII art logo** | Memorable, visually distinctive |
| **Owen story callback** | Connects code to your article's narrative |
| **Key results table** | Immediate proof of success |
| **Architecture diagram** | Shows the pipeline visually |
| **Installation instructions** | Practical usability |
| **Function reference** | For developers who want to dig in |
| **Output examples** | Shows what they'll get |
| **Four tools explanation** | Educational value |
| **Performance metrics** | Credibility |
| **Lessons learned** | Honesty and wisdom |
| **Future improvements** | Transparency |
| **References** | Academic rigor |
| **Contact info** | Professional branding |
| **Memorable quote** | Emotional connection |

---

**This README is ready to go.** It tells the story, shows the results, provides the code, and invites collaboration. Upload it with confidence.
