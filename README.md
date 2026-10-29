# Road Damage Detection with YOLO

An end-to-end road damage detection project that compares multiple lightweight YOLO architectures, tunes their hyperparameters, evaluates them on a held-out test set, and deploys the selected model through a Gradio interface.

The project focuses not only on detection accuracy, but also on practical deployment factors such as inference speed, model size, IoU, and overall efficiency.

## Project Overview

The workflow includes:

```text
Dataset Preparation
        ↓
YOLOv8n / YOLOv10n / YOLO11n
        ↓
Baseline Training
        ↓
Hyperparameter Tuning
        ↓
Validation-Based Model Selection
        ↓
Held-Out Test Evaluation
        ↓
IoU + FPS + Model Size Comparison
        ↓
Deployment Efficiency Ranking
        ↓
Best Model Selection
        ↓
Gradio Interface
```

## Dataset

The dataset was downloaded from Roboflow and converted to YOLO format.

The project contains 8 road-damage classes:

- Alligator cracking
- Edge cracking
- Longitudinal cracking
- Manhole
- Patching
- Pothole
- Rutting
- Transverse cracking

The dataset was separated into training, validation, and held-out test sets.

## Models Compared

Three lightweight YOLO architectures were evaluated:

- YOLOv8n
- YOLOv10n
- YOLO11n

Each model was first trained using baseline settings and then evaluated under multiple hyperparameter configurations.

## Hyperparameter Tuning

The tuning experiments included:

- AdamW optimizer
- SGD optimizer
- Different learning rates
- Different weight decay values
- Different Mosaic augmentation settings

Best validation configurations:

| Model | Best Configuration |
|---|---|
| YOLOv8n | AdamW, LR = 0.001, Weight Decay = 0.0005, Mosaic = 1.0 |
| YOLOv10n | AdamW, LR = 0.0005, Weight Decay = 0.001, Mosaic = 0.5 |
| YOLO11n | AdamW, LR = 0.0005, Weight Decay = 0.001, Mosaic = 0.5 |

## Final Test Results

The best tuned version of each architecture was evaluated on the held-out test set.

| Model | mAP@50 | mAP@50:95 | Precision | Recall | Mean IoU | FPS | Size |
|---|---:|---:|---:|---:|---:|---:|---:|
| YOLO11n | 0.6416 | **0.4171** | 0.5608 | 0.6382 | 0.7728 | 69.27 | **5.25 MB** |
| YOLOv10n | 0.6499 | 0.3974 | **0.6692** | 0.4662 | **0.8074** | 72.15 | 5.55 MB |
| YOLOv8n | **0.6713** | 0.4158 | 0.5395 | **0.6601** | 0.7711 | **79.93** | 6.20 MB |

## Key Findings

The model comparison showed that no single model dominated every metric.

- **YOLO11n** achieved the highest mAP@50:95.
- **YOLOv10n** achieved the highest precision and highest mean matched IoU.
- **YOLOv8n** achieved the highest recall, highest mAP@50, and fastest inference speed.
- **YOLO11n** produced the smallest checkpoint.

## Deployment Model Selection

Rather than selecting the deployment model based on accuracy alone, a project-defined efficiency score was created using:

- 60% mAP@50:95
- 25% FPS
- 15% model compactness

Final deployment ranking:

| Model | Deployment Efficiency Score |
|---|---:|
| YOLOv8n | **0.8117** |
| YOLO11n | 0.7500 |
| YOLOv10n | 0.1702 |

Based on this trade-off between detection performance, speed, and deployment cost, **YOLOv8n was selected as the final deployment model**.

## Final Selected Model

```text
Model: YOLOv8n
mAP@50:95: 0.4158
mAP@50: 0.6713
Recall: 0.6601
FPS: 79.93
Checkpoint Size: 6.20 MB
Deployment Efficiency Score: 0.8117
```

## Evaluation Metrics

The project evaluates each model using:

- Precision
- Recall
- mAP@50
- mAP@50:95
- Mean IoU
- Median IoU
- Inference latency
- FPS
- Model checkpoint size
- Number of model parameters

## Gradio Interface

A lightweight Gradio interface was added for deployment.

The user can:

1. Upload a road image.
2. Select a confidence threshold.
3. Run the trained YOLO model.
4. View detected road damage with bounding boxes.
5. Review predicted damage classes and confidence scores.

## Technologies Used

- Python
- PyTorch
- Ultralytics YOLO
- YOLOv8
- YOLOv10
- YOLO11
- Roboflow
- OpenCV
- Pandas
- NumPy
- Matplotlib
- Gradio
- Kaggle

## Project Structure

```text
road_damage_training_evaluation.ipynb
    ├── Dataset preparation
    ├── Data inspection
    ├── Model training
    ├── Hyperparameter tuning
    ├── Test evaluation
    ├── IoU analysis
    ├── FPS benchmarking
    ├── Model comparison
    └── Deployment model selection

Gradio Interface
    ├── Load best model
    ├── Image upload
    ├── Inference
    └── Detection visualization
```

## Limitations

The current project has several limitations:

- The test set is relatively small.
- Some classes are highly imbalanced.
- Certain road-damage classes have very few test examples.
- Model performance may vary under different road conditions, lighting, camera angles, or image quality.
- The current interface processes uploaded images rather than continuous live road-video streams.
- The system detects visible damage but does not estimate engineering severity or repair cost.

## Future Improvements

Possible extensions include:

- Expanding and balancing the dataset
- Collecting more real-world test images
- Adding damage severity classification
- Real-time road video processing
- GPS-based damage mapping
- Integration with GIS systems
- Automatic road-condition reports
- Mobile or web deployment
- Model quantization for edge deployment
- Testing larger YOLO architectures
- Combining detection with maintenance prioritization

## Objective

The main goal of this project is to demonstrate a complete computer-vision workflow that goes beyond training a single model by comparing architectures, evaluating deployment trade-offs, and converting the final model into a usable detection application.
