# Anomaly Detection Using Isolation Forest and LOF

This project uses **Local Outlier Factor (LOF)** and **Isolation Forest (iForest)** for anomaly detection in network traffic data. By generating meaningful features and utilizing these unsupervised learning techniques, the project aims to identify unusual traffic patterns, which could indicate security threats such as botnet traffic.

## Table of Contents

- [Overview](#overview)
- [Approach](#approach)
  - [Feature Engineering](#feature-engineering)
  - [Local Outlier Factor (LOF)](#local-outlier-factor-lof)
  - [Isolation Forest (iForest)](#isolation-forest-iforest)
- [Datasets](#datasets)
- [Results](#results)
- [Conclusion](#conclusion)

## Overview

With the rise in sophisticated cyber-attacks, identifying anomalies in network traffic has become a key task in cybersecurity. This project leverages unsupervised machine learning algorithms, specifically **Isolation Forest** and **Local Outlier Factor (LOF)**, to detect potential malicious activity in network traffic data. These algorithms are suited for anomaly detection as they don't rely on labels, making them ideal for real-world scenarios where ground truth (labels) is often unavailable.

## Approach

Our approach is built around three major steps: feature engineering, anomaly detection with LOF, and global outlier detection with Isolation Forest. Each step plays a crucial role in creating a comprehensive anomaly detection pipeline.

### Feature Engineering

Feature engineering is a critical part of the anomaly detection process. Here, we extract and refine essential features from raw network traffic data to enhance the accuracy of the algorithms. The features used in this project include:

- **`duration`**: Measures the length of each network session. Anomalous traffic often has unusually short or long durations compared to typical sessions.
- **`total_packets`**: Counts the number of packets sent in each session. High packet counts could indicate large data transfers or potential flooding attacks.
- **`bytes_transferred_total`**: Indicates the total amount of data transferred, a valuable feature for spotting large data exfiltration.
- **`bytes_transferred_source_to_dest`**: Shows the volume of data transferred from the source to the destination, which can reveal unusual data flows.
- **`source_port`** and **`destination_port`**: Represent the ports used by the source and destination devices. Certain port patterns can indicate known botnet communication channels or other malicious activity.

These features are selected to capture various characteristics of network sessions, including traffic volume, direction, and duration. By focusing on these aspects, we enable LOF and Isolation Forest to accurately discern normal from abnormal behavior.

### Local Outlier Factor (LOF)

The **Local Outlier Factor** (LOF) is a density-based anomaly detection method that identifies anomalies based on their distance from neighboring points. It determines how isolated a data point is within its local neighborhood, with isolated (or low-density) points more likely to be anomalies.

#### Why LOF?

1. **Local Density Measurement**: LOF excels at detecting points that are isolated compared to nearby points, making it highly suitable for finding localized anomalies.
2. **Unsupervised Approach**: Since LOF does not rely on labeled data, it’s ideal for real-world situations where we lack ground-truth labels.
3. **Applicability to Various Data Distributions**: LOF works well on datasets with varied distributions, as it only relies on local neighborhood densities rather than assuming a global data distribution.

In this project, we applied LOF as an initial step to identify anomalies within specific subregions of the dataset. This is helpful in cases where certain types of anomalies may only be noticeable within smaller clusters.

### Isolation Forest (iForest)

**Isolation Forest** is an anomaly detection algorithm based on decision trees. Unlike LOF, which considers local density, Isolation Forest isolates anomalies by partitioning the dataset into smaller and smaller segments. Anomalies are typically easier to isolate because they tend to be far from the dense, clustered normal data points.

#### Why Isolation Forest?

1. **Efficient for Large Datasets**: Isolation Forest is computationally efficient, making it suitable for large, high-dimensional datasets.
2. **Robust to Noise**: Its random partitioning method is less sensitive to noise and irregular patterns in the dataset.
3. **Contamination Parameter**: By adjusting the contamination parameter, Isolation Forest can be fine-tuned based on the expected proportion of anomalies, giving it flexibility in detecting anomalies of varying rarity.

Isolation Forest was employed in this project to detect global outliers across the entire dataset. This approach is particularly useful for identifying widespread or less concentrated anomalies that may not be caught by LOF.

## Datasets

The datasets used in this project consist of NetFlow data, a standard format for recording network traffic data. The key fields in the dataset include:

- **Timestamp**: The time the traffic flow was recorded.
- **Duration**: Length of the network session.
- **Protocol**: The protocol used for the connection (e.g., TCP, UDP).
- **Source/Destination IP**: IP addresses for the source and destination of each connection.
- **Source/Destination Ports**: Ports used by the source and destination devices.
- **Total Packets**: Number of packets transferred during the session.
- **Total Bytes**: Amount of data transferred in bytes.

The dataset contains both normal traffic and botnet (malicious) traffic, providing a realistic foundation for anomaly detection.

## Conclusion

This project demonstrates the effectiveness of combining feature engineering with LOF and Isolation Forest for robust anomaly detection in network traffic data. While LOF specializes in finding anomalies based on local density variations, Isolation Forest efficiently isolates global anomalies. Together, these methods provide a well-rounded approach to detecting a variety of anomalies in network traffic, enhancing the ability to identify potential security threats.
