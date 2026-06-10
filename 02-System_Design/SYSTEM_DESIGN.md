# Phase 2: System Design

## High-Level System Architecture

### System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Tesla Stock Price Prediction System       │
│                                                               │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐  │
│  │   Data       │    │   Model      │    │ Evaluation   │  │
│  │   Pipeline   │───▶│ Development  │───▶│ & Metrics    │  │
│  │              │    │              │    │              │  │
│  └──────────────┘    └──────────────┘    └──────────────┘  │
│        │                    │                    │           │
│        ▼                    ▼                    ▼           │
│  ┌──────────────────────────────────────────────────────┐  │
│  │     Streamlit Web Application (Deployment)          │  │
│  │                                                      │  │
│  │  - Interactive Dashboard                           │  │
│  │  - Model Selection                                 │  │
│  │  - Prediction Interface                            │  │
│  │  - Performance Metrics Display                     │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

## System Components

### 1. Data Pipeline Module

**Purpose**: Load, clean, and preprocess stock price data

**Key Functions**:
- Load data from TSLA.csv
- Handle missing values
- Normalize using MinMaxScaler
- Create time-series sequences
- Split train/test data

**Input**: TSLA.csv  
**Output**: Preprocessed training and testing datasets

### 2. Model Development Module

**Purpose**: Build and train deep learning models

**Key Functions**:
- SimpleRNN model implementation
- LSTM model implementation
- Hyperparameter tuning with GridSearchCV
- Model training with early stopping
- Model checkpointing

**Input**: Preprocessed datasets  
**Output**: Trained model weights and history

### 3. Evaluation Module

**Purpose**: Evaluate and compare model performance

**Key Functions**:
- Calculate MSE, RMSE, MAE
- Generate predictions for 1-day, 5-day, 10-day targets
- Create performance comparison reports
- Visualize actual vs predicted prices

**Input**: Trained models, test data  
**Output**: Evaluation metrics and visualizations

### 4. Streamlit Application

**Purpose**: Provide user interface for predictions

**Key Functions**:
- Select model (SimpleRNN or LSTM)
- Choose prediction period (1, 5, or 10 days)
- Display predictions and metrics
- Show performance comparison charts

**Input**: Trained models, user selections  
**Output**: Web interface with predictions

## Data Flow

```
TSLA.csv
   │
   ▼
┌─────────────────────┐
│  Data Loading       │
│  - Read CSV         │
│  - Validate columns │
└─────────────────────┘
   │
   ▼
┌─────────────────────┐
│  Data Cleaning      │
│  - Handle NaN       │
│  - Outlier detection│
└─────────────────────┘
   │
   ▼
┌─────────────────────┐
│  Data Preprocessing │
│  - Normalize        │
│  - Sequence creation│
│  - Train/Test split │
└─────────────────────┘
   │
   ▼
┌──────────────────────────┐
│  Model Training          │
│  - SimpleRNN model       │
│  - LSTM model            │
│  - Hyperparameter tuning │
└──────────────────────────┘
   │
   ▼
┌─────────────────────┐
│  Model Evaluation   │
│  - Predictions      │
│  - Metrics          │
│  - Comparison       │
└─────────────────────┘
   │
   ▼
┌──────────────────────────┐
│  Streamlit Deployment    │
│  - Web Interface         │
│  - Interactive Dashboard │
└──────────────────────────┘
```

## Technology Stack

| Component | Technology | Version |
|-----------|-----------|---------|
| **Programming Language** | Python | 3.8+ |
| **Data Processing** | Pandas | Latest |
| **Numerical Computing** | NumPy | Latest |
| **Deep Learning** | TensorFlow/Keras | 2.x |
| **Visualization** | Matplotlib, Seaborn | Latest |
| **Web Framework** | Streamlit | Latest |
| **Hyperparameter Tuning** | Scikit-learn (GridSearchCV) | Latest |
| **Notebook** | Jupyter | Latest |

## Database Design

### Data Storage Structure

```
├── raw_data/
│   └── TSLA.csv (Original data)
├── processed_data/
│   ├── train_data.npy
│   ├── test_data.npy
│   ├── train_labels.npy
│   └── test_labels.npy
└── models/
    ├── simplernn_model.h5
    ├── lstm_model.h5
    └── scaler.pkl
```

## Training/Test Data Split Strategy

| Metric | Value |
|--------|-------|
| **Total Data** | 100% |
| **Training Set** | 80% |
| **Testing Set** | 20% |
| **Validation** | 20% of training (internal) |
| **Time-Series Window** | n days (configurable) |
| **Prediction Targets** | 1-day, 5-day, 10-day |

## API/Interface Design

### Data Pipeline API

```python
# Load and preprocess data
data, scaler = load_and_preprocess('TSLA.csv')
X_train, y_train, X_test, y_test = prepare_sequences(data, window_size=60)
```

### Model Training API

```python
# Train SimpleRNN model
simplernn_model = build_simplernn(units=50, dropout=0.2)
history_rnn = train_model(simplernn_model, X_train, y_train)

# Train LSTM model
lstm_model = build_lstm(units=50, dropout=0.2)
history_lstm = train_model(lstm_model, X_train, y_train)
```

### Prediction API

```python
# Make predictions
predictions_rnn = simplernn_model.predict(X_test)
predictions_lstm = lstm_model.predict(X_test)

# Inverse transform to original scale
predicted_prices = scaler.inverse_transform(predictions)
```

## System Constraints

- **Memory**: Models run on CPU/GPU with batch processing
- **Processing Time**: Training < 30 minutes
- **Storage**: Model weights < 500 MB
- **Network**: Streamlit deployment requires internet for web access

## Security Considerations

- Input validation for file uploads
- Data sanitization for user inputs
- No sensitive data stored locally
- Model weights protected from unauthorized modification

---

**Document Created**: June 11, 2026  
**Version**: 1.0
