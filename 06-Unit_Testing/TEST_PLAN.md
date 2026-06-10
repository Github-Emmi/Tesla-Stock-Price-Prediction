# Phase 6: Unit Testing

## Unit Testing Plan

### Test Scope

Unit testing focuses on testing individual functions and methods in isolation to ensure they work correctly.

---

## Data Pipeline Module Tests

### Test Suite 1: DataLoader Tests

```python
test_load_valid_csv()
    """Verify CSV file loads correctly"""
    Expected: Data loaded with correct shape and columns
    
test_load_invalid_filepath()
    """Verify error handling for missing file"""
    Expected: FileNotFoundError raised
    
test_validate_required_columns()
    """Verify all required columns present"""
    Expected: Validation passes or raises exception
    
test_data_types()
    """Verify columns have correct data types"""
    Expected: Date (datetime), Price columns (float)
```

### Test Suite 2: DataPreprocessor Tests

```python
test_normalize_data_range()
    """Verify normalized data in [0, 1] range"""
    Expected: Min=0, Max=1
    
test_normalize_preserves_shape()
    """Verify normalization preserves data shape"""
    Expected: Input shape == Output shape
    
test_sequence_creation()
    """Verify time-series sequences created correctly"""
    Expected: Correct dimensions (n_samples, window_size, 1)
    
test_train_test_split()
    """Verify 80/20 train/test split"""
    Expected: len(train) / len(total) ≈ 0.8
    
test_inverse_transform()
    """Verify inverse transform returns original scale"""
    Expected: Inverse values close to original
```

### Test Cases

| Test ID | Test Name | Input | Expected Output | Pass/Fail |
|---------|-----------|-------|-----------------|-----------|
| DL-T1 | Load Valid CSV | Valid TSLA.csv | DataFrame with 7 columns | __ |
| DL-T2 | Handle Missing File | Invalid path | FileNotFoundError | __ |
| DL-T3 | Normalize MinMax | Original prices | Values in [0,1] | __ |
| DL-T4 | Sequence Creation | Raw time series | (n, 60, 1) shape | __ |
| DL-T5 | Train/Test Split | Sequences | 80% train, 20% test | __ |

---

## Model Training Module Tests

### Test Suite 3: SimpleRNNBuilder Tests

```python
test_model_creation()
    """Verify SimpleRNN model created successfully"""
    Expected: Model object with correct layers
    
test_model_compilation()
    """Verify model compiles without errors"""
    Expected: Model in 'compiled' state
    
test_model_prediction()
    """Verify model can generate predictions"""
    Expected: Output shape (batch_size, 1)
    
test_input_shape_handling()
    """Verify model handles correct input shape"""
    Expected: No dimension mismatch errors
```

### Test Suite 4: LSTMBuilder Tests

```python
test_lstm_model_creation()
    """Verify LSTM model created successfully"""
    Expected: LSTM model with specified units
    
test_lstm_compilation()
    """Verify LSTM compiles correctly"""
    Expected: Compilation successful
    
test_lstm_prediction()
    """Verify LSTM generates predictions"""
    Expected: Output shape (batch_size, 1)
    
test_lstm_memory_cells()
    """Verify LSTM maintains cell states"""
    Expected: No state management errors
```

### Test Suite 5: ModelTrainer Tests

```python
test_model_training()
    """Verify model trains without errors"""
    Expected: Training completes, loss decreases
    
test_training_history()
    """Verify training history recorded"""
    Expected: History dict with loss per epoch
    
test_batch_processing()
    """Verify batch processing works"""
    Expected: Training with different batch sizes
    
test_early_stopping()
    """Verify early stopping triggers"""
    Expected: Training stops before max epochs
```

### Test Cases

| Test ID | Test Name | Input | Expected Output | Pass/Fail |
|---------|-----------|-------|-----------------|-----------|
| MT-T1 | Build SimpleRNN | Input shape (60,1) | Model with 5 layers | __ |
| MT-T2 | Build LSTM | Input shape (60,1) | Model with 5 layers | __ |
| MT-T3 | Train Model | X_train, y_train | Loss decreases | __ |
| MT-T4 | Early Stopping | Training data | Stops < max epochs | __ |
| MT-T5 | Prediction Shape | X_test (10, 60, 1) | Output (10, 1) | __ |

---

## Evaluation & Metrics Module Tests

### Test Suite 6: ModelEvaluator Tests

```python
test_mse_calculation()
    """Verify MSE calculation"""
    Expected: MSE ≥ 0
    
test_rmse_calculation()
    """Verify RMSE = sqrt(MSE)"""
    Expected: RMSE ≥ 0
    
test_mae_calculation()
    """Verify MAE calculation"""
    Expected: MAE ≥ 0 and MAE ≤ possible_max
    
test_r_squared_calculation()
    """Verify R² score in [-∞, 1]"""
    Expected: R² ≤ 1
    
test_prediction_accuracy()
    """Verify prediction output format"""
    Expected: Array of correct shape
```

### Test Suite 7: PredictionVisualizer Tests

```python
test_plot_generation()
    """Verify plots generated without errors"""
    Expected: PNG/image files created
    
test_plot_dimensions()
    """Verify plot renders correctly"""
    Expected: Valid image dimensions
    
test_axis_labels()
    """Verify plot has correct labels"""
    Expected: Title, x-label, y-label present
```

### Test Cases

| Test ID | Test Name | Input | Expected Output | Pass/Fail |
|---------|-----------|-------|-----------------|-----------|
| EV-T1 | Calculate MSE | Predictions & actual | MSE value ≥ 0 | __ |
| EV-T2 | Calculate RMSE | Predictions & actual | RMSE = √MSE | __ |
| EV-T3 | Calculate MAE | Predictions & actual | MAE value ≥ 0 | __ |
| EV-T4 | R² Score | Predictions & actual | R² in [-∞, 1] | __ |
| EV-T5 | Generate Plot | Data | PNG image file | __ |

---

## Error Handling Tests

### Test Suite 8: Exception Handling

```python
test_invalid_input_shape()
    """Verify error for wrong input shape"""
    Expected: ValueError raised
    
test_null_data_handling()
    """Verify handling of null values"""
    Expected: Exception or handled gracefully
    
test_out_of_range_values()
    """Verify handling of extreme values"""
    Expected: Clipping or normalization applied
```

---

## Test Execution & Reporting

### Unit Test Framework

**Framework**: pytest  
**Coverage Tool**: pytest-cov

### Test Execution Commands

```bash
# Run all tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=src --cov-report=html

# Run specific test file
pytest tests/test_data_pipeline.py -v

# Run specific test function
pytest tests/test_data_pipeline.py::test_load_valid_csv -v
```

### Expected Test Results

| Module | Total Tests | Expected Pass | Expected Fail | Coverage |
|--------|-------------|---------------|---------------|----------|
| Data Pipeline | 10 | 10 | 0 | > 90% |
| Model Training | 8 | 8 | 0 | > 85% |
| Evaluation | 8 | 8 | 0 | > 90% |
| **Total** | **26** | **26** | **0** | **> 88%** |

### Test Report Template

```
╔════════════════════════════════════════════╗
║       UNIT TESTING REPORT                  ║
╠════════════════════════════════════════════╣
║ Execution Date: [Date]                     ║
║ Total Tests: 26                            ║
║ Passed: 26                                 ║
║ Failed: 0                                  ║
║ Code Coverage: 88.5%                       ║
║ Status: ✅ PASS                            ║
╚════════════════════════════════════════════╝
```

---

**Document Created**: June 11, 2026  
**Version**: 1.0
