# Phase 7: Integration Testing

## Integration Test Plan

### Test Scope

Integration testing validates that different modules work together correctly and data flows properly between components.

---

## Integration Test Suites

### Test Suite 1: Data Pipeline → Model Trainer Integration

```
Test ID: IT-001
Test Name: Data Pipeline to Model Trainer Integration
Objective: Verify preprocessed data integrates correctly with model training

Test Steps:
1. Execute complete data pipeline
2. Get output: X_train, y_train, X_test, y_test, scaler
3. Pass to SimpleRNN trainer
   - Verify input shape compatibility
   - Train model successfully
   - Get training history
4. Pass same data to LSTM trainer
   - Verify input shape compatibility
   - Train model successfully
   - Get training history
5. Compare training histories

Expected Results:
- Data shapes: (n_samples, 60, 1)
- No dimension mismatch errors
- Both models train successfully
- Loss values decrease over epochs
- Training completes without crashes

Test Data: TSLA.csv
Acceptance Criteria: PASS/FAIL
```

### Test Suite 2: Model Trainer → Evaluator Integration

```
Test ID: IT-002
Test Name: Model Trainer to Evaluator Integration
Objective: Verify trained models work with evaluation module

Test Steps:
1. Train SimpleRNN model
2. Save model and predictions
3. Pass to evaluation module
   - Load model
   - Generate predictions on test set
   - Calculate MSE, RMSE, MAE
   - Generate visualizations
4. Train LSTM model (repeat steps)
5. Verify metrics calculated correctly
6. Compare metrics

Expected Results:
- Predictions have correct shape
- Metrics calculated without errors
- Visualizations generated successfully
- Comparison possible

Test Data: Trained models from Phase 5
Acceptance Criteria: PASS/FAIL
```

### Test Suite 3: Data Pipeline → Evaluator Integration (via Model)

```
Test ID: IT-003
Test Name: End-to-End Model Pipeline
Objective: Verify complete flow from data to evaluation

Test Steps:
1. Run complete data pipeline
2. Extract preprocessed data
3. Train both models
4. Generate predictions
5. Calculate evaluation metrics
6. Verify end-to-end flow

Expected Results:
- All components integrate seamlessly
- Data transforms correctly at each step
- Predictions generated successfully
- Metrics calculated correctly
- No data loss or corruption

Acceptance Criteria: PASS/FAIL
```

### Test Suite 4: Evaluation → Visualization Integration

```
Test ID: IT-004
Test Name: Metrics to Visualization Integration
Objective: Verify evaluation metrics display correctly in visualizations

Test Steps:
1. Calculate evaluation metrics
2. Pass to visualization module
3. Generate comparison charts
4. Verify charts contain correct data
5. Validate chart appearance

Expected Results:
- Charts render without errors
- Data points correct
- Legends and labels present
- No rendering artifacts
- Charts are interpretable

Acceptance Criteria: PASS/FAIL
```

### Test Suite 5: Model → Streamlit Integration

```
Test ID: IT-005
Test Name: Trained Models to Streamlit App
Objective: Verify models load and work in Streamlit app

Test Steps:
1. Load trained models in Streamlit
2. Load scaler
3. Accept user input (model selection, period)
4. Generate predictions
5. Calculate metrics
6. Display results
7. Test model switching
8. Test prediction period switching

Expected Results:
- Models load without errors
- Predictions generate on demand
- UI updates correctly
- No state management issues
- Responsive interactions

Acceptance Criteria: PASS/FAIL
```

### Test Suite 6: Data Preprocessing Details Integration

```
Test ID: IT-006
Test Name: Scaler and Data Transformation Integration
Objective: Verify data transformations are reversible

Test Steps:
1. Normalize raw prices
2. Create sequences
3. Make predictions
4. Inverse transform predictions
5. Compare with original scale

Expected Results:
- Inverse transformation successful
- Predicted prices in reasonable range
- Trend preserved
- No numerical errors

Acceptance Criteria: PASS/FAIL
```

---

## Data Flow Integration Tests

### Test Suite 7: Complete Data Flow

```
Test ID: IT-007
Test Name: Complete System Data Flow
Objective: Verify data integrity through entire system

Test Path:
CSV File → Load → Validate → Clean → Normalize → 
Sequence → Split → Train → Predict → Inverse → Display

Validation Points:
✓ Data shape at each step correct
✓ No NaN values propagate
✓ Numerical values within expected ranges
✓ Time-series order preserved
✓ Train/test separation maintained
✓ Predictions reversible to original scale

Acceptance Criteria: PASS/FAIL
```

---

## Module Interface Contracts

### Data Pipeline Interface

```python
# Input: File path
# Output: Tuple of (X_train, y_train, X_test, y_test, scaler)
X_train, y_train, X_test, y_test, scaler = load_and_preprocess('TSLA.csv')

# Expected types:
# X_train: np.ndarray, shape (n_train, 60, 1)
# y_train: np.ndarray, shape (n_train, 1)
# X_test: np.ndarray, shape (n_test, 60, 1)
# y_test: np.ndarray, shape (n_test, 1)
# scaler: MinMaxScaler object
```

### Model Trainer Interface

```python
# Input: Preprocessed data
# Output: Trained model
simplernn_model = train_simplernn(X_train, y_train, X_test, y_test)
lstm_model = train_lstm(X_train, y_train, X_test, y_test)

# Expected:
# simplernn_model: keras.Model object
# lstm_model: keras.Model object
```

### Prediction Interface

```python
# Input: Trained model, test data, scaler
# Output: Predictions in original scale
predictions = model.predict(X_test)
predictions_scaled = scaler.inverse_transform(predictions)

# Expected:
# predictions_scaled: np.ndarray, shape (n_test, 1)
# Values in Tesla stock price range
```

### Evaluation Interface

```python
# Input: Actual and predicted values
# Output: Metrics dictionary
metrics = {
    'mse': calculate_mse(actual, predicted),
    'rmse': calculate_rmse(actual, predicted),
    'mae': calculate_mae(actual, predicted),
    'r_squared': calculate_r_squared(actual, predicted)
}

# Expected:
# All metrics ≥ 0
# Metrics in reasonable ranges for stock prices
```

---

## Error Propagation Testing

### Test Suite 8: Error Handling in Integration

```
Test ID: IT-008
Test Name: Error Propagation Through Modules
Objective: Verify errors are handled gracefully

Failure Scenarios:
1. Missing CSV file
   Expected: Caught at data loading, error message, graceful exit
   
2. Invalid data format
   Expected: Caught during validation, error logged
   
3. Dimension mismatch
   Expected: Caught at model input, error raised
   
4. Numerical issues (NaN, Inf)
   Expected: Detected and handled

Acceptance Criteria: All errors handled appropriately
```

---

## Performance Under Integration

### Test Suite 9: Integration Performance

```
Test ID: IT-009
Test Name: Performance with Integrated Modules
Objective: Verify performance doesn't degrade in integration

Metrics:
- Data pipeline time: < 5 minutes
- Model training time: < 30 minutes
- Prediction generation: < 2 seconds
- Total system execution: < 35 minutes

Acceptance Criteria: All within limits
```

---

## Integration Test Matrix

| Test ID | Component 1 | Component 2 | Status | Notes |
|---------|-----------|-----------|--------|-------|
| IT-001 | Data Pipeline | Model Trainer | __ | __ |
| IT-002 | Model Trainer | Evaluator | __ | __ |
| IT-003 | All Modules | End-to-End | __ | __ |
| IT-004 | Evaluator | Visualization | __ | __ |
| IT-005 | Models | Streamlit | __ | __ |
| IT-006 | Scaler | Transformations | __ | __ |
| IT-007 | Full Flow | Data Integrity | __ | __ |
| IT-008 | All | Error Handling | __ | __ |
| IT-009 | All | Performance | __ | __ |

---

## Sign-Off

| Role | Name | Date | Signature | Approval |
|------|------|------|-----------|----------|
| Integration Test Lead | __ | __ | __ | ☐ Approved |
| Technical Lead | __ | __ | __ | ☐ Approved |

---

**Document Created**: June 11, 2026  
**Version**: 1.0
