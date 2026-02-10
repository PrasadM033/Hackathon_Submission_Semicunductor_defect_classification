#  Edge-Based Semiconductor Defect Classification - 

## 🤖 Problem statement?

Modern semiconductor inspection pipelines depend on centralized cloud inference for defect classification, which is incompatible with real-time manufacturing requirements due to latency, bandwidth overhead, and IP sensitivity of inspection data. Additionally, edge deployment is constrained by limited compute and the lack of large, publicly available defect datasets.

The problem is to design an edge-deployable defect classification system that achieves high accuracy while operating within tight resource constraints and limited labeled data availability.

---------------------------------------

## 🎯 Key Insight

 1. Most inspection images are defect-free, making monolithic multi-class inference inefficient.

 2. Edge constraints require selective, hierarchical inference rather than cloud-scale models.

 3. A two-stage pipeline (Defect Detection → Defect Classification) reduces latency and compute   while preserving accuracy.

 4. Synthetic data can effectively address limited access to labeled semiconductor defect datasets.

 5. ONNX-based deployment enables portable, hardware-agnostic edge inference.

---------------------------------------

## 📝 Proposed Solution

My approach to handle the problem in two stages as:

Stage 1 performs lightweight defect detection, classifying inspection images as Normal or Defective. This stage is optimized for low latency and high recall, ensuring that potential defects are not missed while filtering out the majority of normal samples.

Stage 2 performs fine-grained defect classification and is executed only for images identified as defective in Stage 1. This selective execution significantly reduces average inference cost while maintaining accurate defect categorization.

To address the scarcity of labeled semiconductor defect data, the system incorporates synthetic defect image generation using diffusion-based and procedural techniques. These synthetic samples capture realistic defect morphology and are used to augment training data, improving model robustness.

Both stages are exported to ONNX format and deployed using an edge inference runtime, enabling hardware-agnostic, low-latency inference on CPUs, NPUs, or edge accelerators. Model training and data generation are performed in the cloud, while inference is executed at the edge to ensure data privacy and real-time performance.

---------------------------------------

## System Architecture

![alt text](image-2.png)


---------------------------------------

## Dataset & Data Strategy

Data Sources

The proposed system leverages a hybrid data strategy:

1. Synthetic Defect Images

    Generated using diffusion-based and procedural techniques

    Designed to replicate defect morphology observed in SEM imagery

    Enables scalable data generation without IP exposure

2. Public / Open Datasets (Optional)

    Used only for validation and sanity checks

    Helps improve generalization where domain overlap exists


---------------------------------------


## Defect Classes

The following defect categories were considered:

 1. LER

 2. Open

 3. Short

 4. Void

 5. Malformed via

 6. Crack

 Diffusion Model Approach

A text-conditioned diffusion model was used to generate SEM-like grayscale images.

Key characteristics:

Image size: 224 × 224

Grayscale SEM-style textures

---------------------------------------

## Model Design & Trainin

*** training workflow 
![alt text](image-1.png)

---------------------------------------

## Edge Deployment Strategy

** Two-Stage Execution Flow on Edge

![alt text](image.png)
---------------------------------------

## Results & Evaluation

Stage 1: Defect Detection Performance

Objective: Maximize recall to avoid missing defects.

Metric	Value
Accuracy	        : 80%
Recall (Defect)	    : High (target > 95%)
Precision	        : High
False Negative Rate	: Low

Stage 2: Defect Classification Performance

Objective: Correctly classify defect type for flagged samples.

Metric	Value
Overall Accuracy	   : ~65%
Average F1-score	   : Moderate
Challenging Classes	   : short, open, crack


Compared to a single multi-class model, the two-stage pipeline significantly reduces average inference cost.
---------------------------------------

 ## 📁 Project Structure

Hackathon_Submission_Semicunductor_defect_classification/
│
├── README.md                          # Main project documentation
├── Model_description.ipynb            # Notebook explaining model and approach
│
├── sem-dataset/                       # Dataset folder
│   └── (SEM defect dataset files)     # Synthetic / real SEM images
│
├── output/                           # Output results
│   └── (onnx files)
│
└── Doc/
    └── (Detailed description on the project)

---------------------------------------

## Limitations & Future Work

1. Size of model for both stage 1 and 2 is around 5MB, next goal is to brig it down by quantization.

2. Take care of accuracy vs latency trade off

---------------------------------------

