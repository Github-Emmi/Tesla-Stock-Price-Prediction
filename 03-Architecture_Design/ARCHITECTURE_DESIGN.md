# Phase 3: Architecture Design

## Deep Learning Model Architecture

### SimpleRNN Model Architecture

```
Input Layer: (batch_size, 60, 1)
    │
    ▼
SimpleRNN Layer 1: 50 units
    - Input shape: (60, 1)
    - Return sequences: True
    │
    ▼
Dropout Layer: 0.2
    - Prevent overfitting
    │
    ▼
SimpleRNN Layer 2: 25 units
    - Return sequences: False
    │
    ▼
Dropout Layer: 0.2
    │
    ▼
Dense Layer: 1 unit
    - Activation: Linear
    - Output shape: (batch_size, 1)
```

### LSTM Model Architecture

```
Input Layer: (batch_size, 60, 1)
    │
    ▼
LSTM Layer 1: 50 units
    - Input shape: (60, 1)
    - Return sequences: True
    │
    ▼
Dropout Layer: 0.2
    │
    ▼
LSTM Layer 2: 25 units
    - Return sequences: False
    │
    ▼
Dropout Layer: 0.2
    │
    ▼
Dense Layer: 1 unit
    - Activation: Linear
    - Output shape: (batch_size, 1)
```

## Model Comparison

| Aspect | SimpleRNN | LSTM |
|--------|-----------|------|
| **Architecture** | Single hidden state | Hidden + Cell state |
| **Memory** | Short-term memory | Long-term memory |
| **Gradient Flow** | Prone to vanishing/exploding | Better gradient propagation |
| **Training Time** | Faster | Slower |
| **Accuracy** | Lower on long sequences | Higher on long sequences |
| **Use Case** | Short-term dependencies | Long-term dependencies |

## Hyperparameter Search Space

### GridSearchCV Parameters

```python
param_grid = {
    'lstm_units': [32, 50, 100],
    'dropout_rate': [0.1, 0.2, 0.3],
    'learning_rate': [0.001, 0.01, 0.1],
    'batch_size': [16, 32, 64],
    'epochs': [50, 100, 150]
}
```

### Best Hyperparameters (Expected)

| Parameter | SimpleRNN | LSTM |
|-----------|-----------|------|
| **Units (Layer 1)** | 50 | 50 |
| **Units (Layer 2)** | 25 | 25 |
| **Dropout Rate** | 0.2 | 0.2 |
| **Learning Rate** | 0.001 | 0.001 |
| **Optimizer** | Adam | Adam |
| **Batch Size** | 32 | 32 |
| **Epochs** | 100 | 100 |
| **Early Stopping Patience** | 10 | 10 |

## Training Configuration

### Loss Function
- **Function**: Mean Squared Error (MSE)
- **Reason**: Standard for regression tasks
- **Formula**: MSE = (1/n) × Σ(y_true - y_pred)²

### Optimizer
- **Algorithm**: Adam (Adaptive Moment Estimation)
- **Learning Rate**: 0.001
- **Momentum**: Default (β1=0.9, β2=0.999)

### Regularization Strategy
- **Dropout Rate**: 0.2 (20% of neurons dropped)
- **Early Stopping**: Monitor validation loss, patience=10
- **ModelCheckpoint**: Save best weights based on validation loss

### Data Augmentation (if needed)
- Time-series specific augmentation
- Noise injection for robustness
- Time shifting

## Feature Engineering

### Input Features
- **Feature**: Adjusted Closing Price (Adj Close)
- **Normalization**: MinMaxScaler (0-1 range)
- **Window Size**: 60 days (configurable)
- **Prediction Window**: 1-day, 5-day, 10-day

### Time-Series Sequence Creation

```
Example with window_size=3:
Raw sequence: [100, 102, 105, 108, 110]

Create:
X = [
    [100, 102, 105],
    [102, 105, 108],
    [105, 108, 110]
]

y = [108, 110, ...]
```

## Prediction Strategy

### 1-Day Ahead Prediction
- Predict next day's closing price
- Input: Last 60 days
- Output: Day 61 price

### 5-Day Ahead Prediction
- Predict 5 days in advance
- Use iterative approach (recursive prediction)
- Input: Last 60 days → Predict day 61-65

### 10-Day Ahead Prediction
- Predict 10 days in advance
- Use iterative approach with accumulated predictions
- Input: Last 60 days → Predict day 61-70

## Performance Targets

| Metric | Target |
|--------|--------|
| **MSE** | < 100 |
| **RMSE** | < 10 |
| **MAE** | < 7 |
| **R² Score** | > 0.85 |
| **Training Accuracy** | > 90% |
| **Test Accuracy** | > 85% |

## Resource Requirements

| Resource | Requirement |
|----------|-------------|
| **RAM** | 4 GB minimum, 8 GB recommended |
| **GPU** | Optional (NVIDIA GPU recommended for faster training) |
| **Storage** | 2 GB for data + models |
| **CPU Cores** | 4 cores minimum |
| **Inference Time** | < 2 seconds per prediction |

## Error Handling & Edge Cases

### Handling Missing Data
```
Strategy:
1. Check for NaN values
2. Use interpolation (linear/forward-fill)
3. Remove rows with excessive missing values
4. Log number of handled missing values
```

### Handling Outliers
```
Strategy:
1. Calculate mean ± 3σ range
2. Flag outliers for review
3. Option to remove or cap outliers
4. Document outlier handling in report
```

### Prediction on Unseen Data
```
Strategy:
1. Use trained scaler for normalization
2. Ensure input sequence length matches training (60 days)
3. Validate prediction ranges
4. Flag unexpected prediction values
```

## Scalability Considerations

- Batch processing for large datasets
- Model serialization for quick loading
- Lazy loading for data
- Caching mechanisms for repeated predictions

---

**Document Created**: June 11, 2026  
**Version**: 1.0
