# Phase 4: Module Design

## Module Overview

The Tesla Stock Price Prediction system is composed of four primary modules:

```
┌──────────────────────────────┐
│  Main Entry Point            │
│  (main.py / Notebook)        │
└────────────┬─────────────────┘
             │
    ┌────────┼────────┬─────────────┐
    │        │        │             │
    ▼        ▼        ▼             ▼
┌────────┐┌──────┐┌──────────┐┌──────────┐
│Data    ││Model ││Evaluation││Streamlit │
│Pipeline││Trainer││Metrics   ││Interface │
│Module  ││Module ││Module    ││Module    │
└────────┘└──────┘└──────────┘└──────────┘
```

---

## Module 1: Data Pipeline Module

### Module Purpose
Load, validate, clean, and preprocess stock price data

### Key Components

#### 1.1 DataLoader Class
```python
class DataLoader:
    """Load Tesla stock price data from CSV"""
    
    Methods:
    - load_data(filepath)
    - validate_columns()
    - get_summary_stats()
    - handle_missing_values()
```

#### 1.2 DataPreprocessor Class
```python
class DataPreprocessor:
    """Preprocess and normalize data"""
    
    Methods:
    - normalize_data(data, method='minmax')
    - create_sequences(data, window_size)
    - split_train_test(X, y, test_size=0.2)
    - inverse_transform(scaled_data)
```

### Input/Output Specifications

| Aspect | Details |
|--------|---------|
| **Input** | TSLA.csv file path |
| **Output** | Normalized X_train, y_train, X_test, y_test |
| **Data Shape** | (n_samples, sequence_length, 1) |
| **Normalization** | MinMaxScaler (0, 1) |
| **Window Size** | 60 days (default, configurable) |

### Error Handling

- File not found exception
- Invalid column names
- Data type validation
- Missing value detection

### Data Pipeline Workflow

```
┌─────────────┐
│ Load TSLA   │
│ CSV File    │
└──────┬──────┘
       │
       ▼
┌─────────────────────┐
│ Validate Columns    │
│ Check Data Types    │
└──────┬──────────────┘
       │
       ▼
┌──────────────────────┐
│ Handle Missing       │
│ Values               │
└──────┬───────────────┘
       │
       ▼
┌──────────────────────┐
│ Calculate            │
│ Statistics           │
└──────┬───────────────┘
       │
       ▼
┌──────────────────────┐
│ Normalize Data       │
│ MinMaxScaler         │
└──────┬───────────────┘
       │
       ▼
┌──────────────────────┐
│ Create Time-Series   │
│ Sequences            │
└──────┬───────────────┘
       │
       ▼
┌──────────────────────┐
│ Split Train/Test     │
│ 80/20 Ratio          │
└──────┴───────────────┘
```

---

## Module 2: Model Training Module

### Module Purpose
Build, compile, and train SimpleRNN and LSTM models

### Key Components

#### 2.1 SimpleRNNBuilder Class
```python
class SimpleRNNBuilder:
    """Build and configure SimpleRNN model"""
    
    Methods:
    - build_model(input_shape, units, dropout)
    - compile_model(optimizer, loss)
    - get_architecture_summary()
```

#### 2.2 LSTMBuilder Class
```python
class LSTMBuilder:
    """Build and configure LSTM model"""
    
    Methods:
    - build_model(input_shape, units, dropout)
    - compile_model(optimizer, loss)
    - get_architecture_summary()
```

#### 2.3 ModelTrainer Class
```python
class ModelTrainer:
    """Train models with callbacks"""
    
    Methods:
    - train(model, X_train, y_train, epochs, batch_size)
    - add_early_stopping(patience)
    - add_checkpoint(filepath)
    - get_training_history()
```

#### 2.4 HyperparameterTuner Class
```python
class HyperparameterTuner:
    """Tune hyperparameters using GridSearchCV"""
    
    Methods:
    - create_param_grid()
    - tune_hyperparameters()
    - get_best_params()
    - get_best_model()
```

### Model Architecture Specifications

#### SimpleRNN Model
```
Layer 1: SimpleRNN(units=50, return_sequences=True)
Layer 2: Dropout(0.2)
Layer 3: SimpleRNN(units=25, return_sequences=False)
Layer 4: Dropout(0.2)
Layer 5: Dense(1, activation='linear')
```

#### LSTM Model
```
Layer 1: LSTM(units=50, return_sequences=True)
Layer 2: Dropout(0.2)
Layer 3: LSTM(units=25, return_sequences=False)
Layer 4: Dropout(0.2)
Layer 5: Dense(1, activation='linear')
```

### Training Configuration

| Parameter | Value |
|-----------|-------|
| **Optimizer** | Adam |
| **Loss Function** | MSE |
| **Learning Rate** | 0.001 |
| **Batch Size** | 32 |
| **Epochs** | 100 |
| **Validation Split** | 0.2 |
| **Early Stopping Patience** | 10 |

### Output Specifications

| Aspect | Details |
|--------|---------|
| **Output** | Trained model (h5 format) + Training history |
| **Model Weights** | Saved and reloadable |
| **Training Log** | Metrics per epoch |

---

## Module 3: Evaluation & Metrics Module

### Module Purpose
Evaluate model performance and generate comparison metrics

### Key Components

#### 3.1 ModelEvaluator Class
```python
class ModelEvaluator:
    """Evaluate model performance on test data"""
    
    Methods:
    - predict(model, X_test)
    - calculate_mse()
    - calculate_rmse()
    - calculate_mae()
    - calculate_r_squared()
```

#### 3.2 PredictionVisualizer Class
```python
class PredictionVisualizer:
    """Visualize predictions vs actual values"""
    
    Methods:
    - plot_predictions(actual, predicted)
    - plot_comparison(simplernn_pred, lstm_pred, actual)
    - plot_training_history(history)
    - plot_metrics_comparison()
```

#### 3.3 ModelComparator Class
```python
class ModelComparator:
    """Compare SimpleRNN vs LSTM performance"""
    
    Methods:
    - compare_metrics()
    - generate_comparison_report()
    - create_performance_table()
```

### Evaluation Metrics

| Metric | Formula | Interpretation |
|--------|---------|-----------------|
| **MSE** | Σ(y_true - y_pred)² / n | Lower is better |
| **RMSE** | √MSE | Same units as target |
| **MAE** | Σ\|y_true - y_pred\| / n | Average absolute error |
| **R² Score** | 1 - (SS_res / SS_tot) | 1.0 is perfect fit |

### Output Specifications

| Output | Format |
|--------|--------|
| **Evaluation Metrics** | JSON/CSV |
| **Visualizations** | PNG images |
| **Comparison Report** | Markdown/PDF |
| **Performance Tables** | HTML/Excel |

---

## Module 4: Streamlit Interface Module

### Module Purpose
Provide interactive web interface for predictions and metrics display

### Key Components

#### 4.1 StreamlitApp Class
```python
class StreamlitApp:
    """Main Streamlit application"""
    
    Methods:
    - setup_page()
    - load_models()
    - render_sidebar()
    - render_main_content()
```

#### 4.2 UIComponents Class
```python
class UIComponents:
    """Reusable UI components"""
    
    Methods:
    - model_selector()
    - prediction_period_selector()
    - display_metrics()
    - display_charts()
```

### User Interface Layout

```
Sidebar:
├── Title: "Tesla Stock Price Prediction"
├── Model Selection
│   ├── SimpleRNN
│   └── LSTM
├── Prediction Period
│   ├── 1-Day
│   ├── 5-Day
│   └── 10-Day
└── Buttons
    ├── Make Prediction
    └── Compare Models

Main Panel:
├── Current Prediction
│   ├── Predicted Price
│   ├── Confidence Level
│   └── Historical Chart
├── Performance Metrics
│   ├── MSE
│   ├── RMSE
│   └── MAE
├── Model Comparison
│   ├── Metrics Table
│   └── Comparison Charts
└── Additional Info
    ├── Model Architecture
    └── Data Statistics
```

### Features

- **Real-time Predictions**: Submit inputs and get predictions instantly
- **Model Comparison**: Side-by-side comparison of SimpleRNN vs LSTM
- **Interactive Charts**: Plotly-based interactive visualizations
- **Performance Dashboard**: Key metrics and statistics display
- **Download Reports**: Export results as CSV/PDF

---

## Inter-Module Communication

```
Main Script
    │
    ├─→ DataPipeline
    │       └─→ Returns: X_train, y_train, X_test, y_test
    │
    ├─→ ModelTrainer
    │       ├─→ Input: X_train, y_train
    │       └─→ Returns: trained_model_rnn, trained_model_lstm
    │
    ├─→ ModelEvaluator
    │       ├─→ Input: trained_model, X_test, y_test
    │       └─→ Returns: metrics, predictions
    │
    └─→ StreamlitApp
            ├─→ Input: trained_models, scaler
            └─→ Returns: web_interface
```

---

## Module Dependencies

```python
# Data Pipeline Module
- pandas
- numpy
- scikit-learn (MinMaxScaler)

# Model Training Module
- tensorflow/keras
- scikit-learn (GridSearchCV)
- numpy

# Evaluation Module
- sklearn.metrics
- matplotlib
- seaborn
- plotly

# Streamlit Module
- streamlit
- plotly
- pandas
```

---

## Testing Strategy per Module

| Module | Unit Tests | Integration Tests |
|--------|------------|------------------|
| **Data Pipeline** | Data loading, validation, preprocessing | End-to-end data flow |
| **Model Training** | Model building, compilation, training | Training pipeline |
| **Evaluation** | Metrics calculation, visualization | Full evaluation pipeline |
| **Streamlit** | Component rendering, user interactions | Full application flow |

---

**Document Created**: June 11, 2026  
**Version**: 1.0
