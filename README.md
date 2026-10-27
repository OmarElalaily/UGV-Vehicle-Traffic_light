# UGV Autonomous Vehicular Traffic Light Detection (YOLOv8)
Deep learning model for interpreting **Vehicular Traffic Light signals** to govern UGV autonomous speed and stopping behavior.
---

### Project Objectives

* **Goal:** Detect and classify the current state of vehicular traffic lights under varied real-world conditions.
* **Function:** Implement a system capable of differentiating the **four critical states** (Red, Yellow, Green, Off) to ensure safe and rule-compliant UGV operation.

### Methodology Highlights

* **Architecture:** Utilizes the **YOLOv8** object detection framework.
* **Data Source:** Trained on the **Bosch Small Traffic Lights Dataset**, a specialized dataset designed for advanced automotive vision tasks, ensuring high relevance and quality. 
* **State Classification:** The model is trained to classify the following four critical traffic light states:
    * **Red** (Stop)
    * **Yellow** (Caution/Prepare to Stop)
    * **Green** (Proceed)
    * **Off** (Requires interpretation as a sign or non-functional)

### Dataset Source

* **Bosch Small Traffic Lights Dataset:** [https://hci.iwr.uni-heidelberg.de/content/bosch-small-traffic-lights-dataset](https://hci.iwr.uni-heidelberg.de/content/bosch-small-traffic-lights-dataset)
    * *Note: This repository contains the code and configuration used for the training, validation, and fine-tuning splits on the downloaded data.*

### Training Setup

* **Initialization:** trained **from scratch** (model built from `yolov8.yaml`, random weights), then **fine-tuned** from the best checkpoint.
* **Hardware:** multi-GPU data-parallel training on a Linux GPU server (2 GPUs).
* **Schedule:** 100 epochs, image size 1280, batch 8, cosine learning-rate decay (1e-4 to 1e-5), early-stopping patience 20; fine-tuning 20 epochs.
* **Evaluation:** separate train / validation / test splits; curves, confusion matrices and predictions are in `output_runs/` and `fine_tune_output_runs/`.

### Results (validation set)

| Stage | mAP@50 | mAP@50-95 | Precision | Recall |
|---|---|---|---|---|
| Trained from scratch (best epoch) | 0.751 | 0.489 | 0.831 | 0.665 |
| After fine-tuning | 0.746 | 0.487 | **0.886** | 0.638 |

**What limits it:** Bosch traffic lights are often only a few pixels tall, and the Yellow and Off states are rare, so recall is the weak point. Fine-tuning raised precision (fewer false lights) at a small cost in recall. Next steps: higher input resolution or tiling for small objects, and class rebalancing for Yellow/Off.

### Inference Results

Below are samples of the model running on unseen validation data:

**1. Red Light Detection**

![Detection of red light](./images/red.jpg)

**2. Green Light Detection**

![Detection of Green Light](./images/green.jpg)

<hr>