# Phase 1: Planning & Requirements Analysis

## Project Requirements Documentation

### 1. Functional Requirements

#### 1.1 Data Processing Requirements
- **FR1**: System shall load Tesla stock data (TSLA.csv) with columns: Date, Open, High, Low, Close, Adj Close, Volume
- **FR2**: System shall identify and handle missing values in dataset
- **FR3**: System shall normalize data using MinMaxScaler (0-1 range)
- **FR4**: System shall create time-series sequences with configurable window size (n days)

#### 1.2 Model Development Requirements
- **FR5**: System shall implement SimpleRNN model with configurable architecture
- **FR6**: System shall implement LSTM model with configurable architecture
- **FR7**: System shall support 1-day, 5-day, and 10-day price predictions
- **FR8**: System shall implement GridSearchCV for hyperparameter tuning
- **FR9**: System shall incorporate dropout layers for overfitting prevention
- **FR10**: System shall support early stopping and ModelCheckpoint during training

#### 1.3 Evaluation Requirements
- **FR11**: System shall calculate Mean Squared Error (MSE) for model evaluation
- **FR12**: System shall calculate Root Mean Squared Error (RMSE)
- **FR13**: System shall calculate Mean Absolute Error (MAE)
- **FR14**: System shall generate comparison metrics between SimpleRNN and LSTM
- **FR15**: System shall visualize actual vs predicted stock prices

#### 1.4 Deployment Requirements
- **FR16**: System shall provide Streamlit web interface for predictions
- **FR17**: System shall allow users to input prediction periods (1, 5, or 10 days)
- **FR18**: System shall display model performance metrics on dashboard

### 2. Non-Functional Requirements

#### 2.1 Performance Requirements
- **NFR1**: Model training shall complete within acceptable timeframe (< 30 minutes)
- **NFR2**: Prediction inference time shall be < 2 seconds
- **NFR3**: Streamlit dashboard shall load within < 5 seconds

#### 2.2 Reliability Requirements
- **NFR4**: Model predictions shall have accuracy > 85% on test set
- **NFR5**: System shall handle edge cases (missing data, outliers)

#### 2.3 Code Quality Requirements
- **NFR6**: Code shall follow PEP 8 Python standards
- **NFR7**: Code shall include comprehensive comments and docstrings
- **NFR8**: Code shall be modular and reusable

#### 2.4 Documentation Requirements
- **NFR9**: Jupyter Notebook shall include all analysis steps and explanations
- **NFR10**: Technical documentation shall explain model architectures
- **NFR11**: User documentation shall include setup and usage instructions

### 3. Data Requirements

| Requirement | Details |
|------------|---------|
| **Data Source** | TSLA.csv (Tesla historical stock data) |
| **Target Variable** | Adj Close price |
| **Feature Columns** | Date, Open, High, Low, Close, Adj Close, Volume |
| **Time Period** | Full dataset history |
| **Data Quality** | Handle missing/null values appropriately |
| **Train/Test Split** | 80% training, 20% testing |
| **Scaling** | MinMaxScaler normalization |

### 4. Model Requirements

| Model | Requirement |
|-------|------------|
| **SimpleRNN** | Configurable layers, dropout rate, units |
| **LSTM** | Configurable LSTM units, dropout, recurrent dropout |
| **Hyperparameters** | GridSearchCV tuning for units, dropout, learning rate |
| **Optimizer** | Adam, SGD, or Mini-Batch optimizers |
| **Loss Function** | Mean Squared Error (MSE) |
| **Prediction Targets** | 1-day, 5-day, 10-day forecast |

### 5. Deliverables

- [ ] Jupyter Notebook with full analysis and model implementation
- [ ] Trained SimpleRNN model (saved weights)
- [ ] Trained LSTM model (saved weights)
- [ ] Comparison report: SimpleRNN vs LSTM
- [ ] Streamlit web application
- [ ] Detailed technical report
- [ ] Documentation and SOPs
- [ ] Video walkthrough

### 6. Success Criteria

- ✅ Both SimpleRNN and LSTM models trained successfully
- ✅ Model comparison metrics calculated and visualized
- ✅ Predictions generated for 1-day, 5-day, 10-day targets
- ✅ MSE < acceptable threshold on test set
- ✅ Streamlit application functional and deployable
- ✅ All documentation complete and professional
- ✅ Code is reproducible and well-documented

### 7. Constraints & Assumptions

**Constraints:**
- Submission: Production ready software ready for use.
- External APIs for real-time data (use historical dataset for fallback)
- Python 3.8 or higher

**Assumptions:**
- Past stock prices are indicative of future trends
- No significant market disruptions in prediction period
- Data quality is sufficient for model training
- 30-day historical data is adequate for initial predictions

### 8. Stakeholders

| Stakeholder | Role | Responsibilities |
|------------|------|------------------|
| Data Scientist | Project Lead | Model development, analysis |
| ML Engineer | Development | Code implementation, optimization |
| QA Engineer | Testing | Model validation, testing |
| Business Analyst | Oversight | Requirements verification |

---

## Traceability Matrix

| Requirement ID | Requirement | Validation Method | Phase |
|---|---|---|---|
| FR1 | Load Tesla data | Unit test | Unit Testing |
| FR5 | Implement SimpleRNN | Code review + Unit test | Implementation |
| FR6 | Implement LSTM | Code review + Unit test | Implementation |
| FR12 | Calculate RMSE | Unit test + System test | System Testing |
| NFR9 | Jupyter Notebook | Review + Acceptance | Acceptance Testing |

