# Phase 8: System Testing

## System Test Plan

### Test Scope

System testing validates the complete integrated system against functional and non-functional requirements.

---

## Functional Testing

### Test Suite 1: End-to-End Data Pipeline

```
Test ID: ST-F1-001
Test Name: Complete Data Pipeline Flow
Objective: Verify data flows correctly from input to model-ready format

Steps:
1. Load TSLA.csv file
2. Validate data structure
3. Clean missing values
4. Normalize using MinMaxScaler
5. Create time-series sequences
6. Split train/test data

Expected Result: 
- X_train shape: (n_train, 60, 1)
- y_train shape: (n_train, 1)
- X_test shape: (n_test, 60, 1)
- y_test shape: (n_test, 1)
- No exceptions raised

Acceptance Criteria: PASS/FAIL
```

### Test Suite 2: SimpleRNN Model Training & Prediction

```
Test ID: ST-F2-001
Test Name: SimpleRNN Training Pipeline
Objective: Verify SimpleRNN model trains and predicts correctly

Steps:
1. Build SimpleRNN architecture
2. Compile with Adam optimizer
3. Train on X_train, y_train
4. Monitor validation loss
5. Apply early stopping
6. Generate predictions on X_test

Expected Result:
- Training loss decreases
- Final training loss < 100
- Validation loss monitoring active
- Predictions generated with correct shape
- Prediction values in reasonable range

Acceptance Criteria: PASS/FAIL
```

### Test Suite 3: LSTM Model Training & Prediction

```
Test ID: ST-F3-001
Test Name: LSTM Training Pipeline
Objective: Verify LSTM model trains and predicts correctly

Steps:
1. Build LSTM architecture
2. Compile with Adam optimizer
3. Train on X_train, y_train
4. Monitor validation loss
5. Apply early stopping
6. Generate predictions on X_test

Expected Result:
- Training loss decreases
- Final training loss < 100
- LSTM performs better or equal to SimpleRNN
- Predictions generated with correct shape

Acceptance Criteria: PASS/FAIL
```

### Test Suite 4: Model Comparison

```
Test ID: ST-F4-001
Test Name: SimpleRNN vs LSTM Performance Comparison
Objective: Verify models can be compared on multiple metrics

Steps:
1. Get SimpleRNN predictions
2. Get LSTM predictions
3. Calculate MSE for both
4. Calculate RMSE for both
5. Calculate MAE for both
6. Generate comparison table
7. Determine winner (lower MSE)

Expected Result:
- SimpleRNN MSE: Within 200
- LSTM MSE: Within 200
- RMSE = √MSE for both
- MAE < 15 for both
- Comparison table generated
- Winner identified

Acceptance Criteria: PASS/FAIL
```

### Test Suite 5: Prediction for Different Horizons

```
Test ID: ST-F5-001
Test Name: 1-Day, 5-Day, 10-Day Predictions
Objective: Verify models generate predictions for all time horizons

Steps:
1. Prepare data for 1-day prediction
2. Generate 1-day ahead price
3. Prepare data for 5-day prediction
4. Generate 5-day ahead prices (iterative)
5. Prepare data for 10-day prediction
6. Generate 10-day ahead prices (iterative)
7. Validate prediction ranges

Expected Result:
- 1-day prediction: Single value
- 5-day predictions: 5 values
- 10-day predictions: 10 values
- All predictions in reasonable range
- Prices show trend consistency

Acceptance Criteria: PASS/FAIL
```

---

## Non-Functional Testing

### Test Suite 6: Performance Testing

```
Test ID: ST-NF-001
Test Name: Model Training Performance
Objective: Verify model training completes within time limits

Criteria:
- SimpleRNN training: < 30 minutes
- LSTM training: < 30 minutes
- Memory usage: < 4 GB
- CPU utilization: < 80%

Expected Result:
- Training completes within 30 minutes
- Memory remains stable
- No out-of-memory errors

Acceptance Criteria: PASS/FAIL
```

### Test Suite 7: Inference Performance

```
Test ID: ST-NF-002
Test Name: Prediction Inference Speed
Objective: Verify predictions generate quickly

Criteria:
- Single prediction: < 2 seconds
- Batch prediction (100 samples): < 5 seconds
- Streamlit page load: < 5 seconds

Expected Result:
- Inference time within limits
- No timeout errors
- Responsive UI

Acceptance Criteria: PASS/FAIL
```

### Test Suite 8: Scalability Testing

```
Test ID: ST-NF-003
Test Name: System Scalability
Objective: Verify system handles larger datasets

Test Cases:
- Test with 50% more data
- Test with different batch sizes (16, 32, 64)
- Test with longer sequences (100 days instead of 60)

Expected Result:
- System handles variations
- Performance acceptable
- No memory issues

Acceptance Criteria: PASS/FAIL
```

---

## Integration Testing

### Test Suite 9: Model & Evaluation Integration

```
Test ID: ST-I-001
Test Name: Model Output to Evaluation Pipeline
Objective: Verify trained models integrate with evaluation module

Steps:
1. Train SimpleRNN
2. Export predictions
3. Pass to evaluation module
4. Calculate metrics
5. Verify metric correctness

Expected Result:
- Metrics calculated successfully
- Values within expected ranges
- No integration errors

Acceptance Criteria: PASS/FAIL
```

### Test Suite 10: Data Pipeline & Model Integration

```
Test ID: ST-I-002
Test Name: Data Pipeline to Model Training
Objective: Verify preprocessed data works with models

Steps:
1. Run data pipeline
2. Pass output to SimpleRNN trainer
3. Pass output to LSTM trainer
4. Verify training succeeds

Expected Result:
- No shape mismatch errors
- Training proceeds normally
- Models converge

Acceptance Criteria: PASS/FAIL
```

---

## Streamlit Application Testing

### Test Suite 11: UI Functionality

```
Test ID: ST-UI-001
Test Name: Streamlit Interface Components
Objective: Verify all UI elements work correctly

Test Cases:
- Model selector dropdown works
- Prediction period selector works
- Make Prediction button triggers action
- Results display correctly
- Charts render properly
- Comparison table displays metrics

Expected Result:
- All components functional
- No rendering errors
- Data displays correctly

Acceptance Criteria: PASS/FAIL
```

### Test Suite 12: User Workflows

```
Test ID: ST-UI-002
Test Name: End-to-End User Workflow
Objective: Simulate complete user interactions

Workflow:
1. User opens Streamlit app
2. Selects SimpleRNN model
3. Chooses 1-day prediction
4. Clicks "Make Prediction"
5. Views results and metrics
6. Switches to LSTM model
7. Compares models

Expected Result:
- No errors during workflow
- All features accessible
- Results accurate and displayed

Acceptance Criteria: PASS/FAIL
```

---

## Regression Testing

### Test Suite 13: Model Stability

```
Test ID: ST-REG-001
Test Name: Model Output Consistency
Objective: Verify models produce consistent predictions

Steps:
1. Train model (seed = 42)
2. Make predictions
3. Retrain model (same seed)
4. Make predictions
5. Compare outputs

Expected Result:
- Predictions identical (with same seed)
- Model weights reproducible
- No random variations

Acceptance Criteria: PASS/FAIL
```

---

## Test Execution Matrix

| Test Suite | Test ID | Test Name | Status | Notes |
|------------|---------|-----------|--------|-------|
| E2E Data | ST-F1 | Data Pipeline | __ | __ |
| SimpleRNN | ST-F2 | RNN Training | __ | __ |
| LSTM | ST-F3 | LSTM Training | __ | __ |
| Comparison | ST-F4 | Model Compare | __ | __ |
| Horizons | ST-F5 | Multi-horizon | __ | __ |
| Performance | ST-NF1 | Training Speed | __ | __ |
| Inference | ST-NF2 | Inference Speed | __ | __ |
| Scalability | ST-NF3 | Scaling | __ | __ |
| Integration1 | ST-I1 | Model-Eval | __ | __ |
| Integration2 | ST-I2 | Data-Model | __ | __ |
| UI | ST-UI1 | Components | __ | __ |
| Workflow | ST-UI2 | User Journey | __ | __ |
| Regression | ST-REG1 | Stability | __ | __ |

---

## Success Criteria

- ✅ All functional tests pass
- ✅ All non-functional tests pass
- ✅ Performance within limits
- ✅ No critical bugs
- ✅ User workflows complete successfully
- ✅ System is production-ready

---

## Sign-Off

| Role | Name | Date | Signature |
|------|------|------|-----------|
| QA Lead | __ | __ | __ |
| Tech Lead | __ | __ | __ |
| Project Manager | __ | __ | __ |

---

**Document Created**: June 11, 2026  
**Version**: 1.0
