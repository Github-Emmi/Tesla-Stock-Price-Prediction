# Phase 10: Deployment & Maintenance

## Deployment Guide

### Pre-Deployment Checklist

- [ ] All tests passed (unit, integration, system, acceptance)
- [ ] Code reviewed and approved
- [ ] Documentation complete
- [ ] Models trained and validated
- [ ] Streamlit app tested
- [ ] Performance meets requirements
- [ ] Security review completed
- [ ] Backup of code and models created

---

## Local Deployment

### 1. Environment Setup

```bash
# Clone repository
git clone https://github.com/Github-Emmi/Tesla-Stock-Price-Prediction.git
cd Tesla-Stock-Price-Prediction

# Create virtual environment
python -m venv venv

# Activate virtual environment
# On macOS/Linux:
source venv/bin/activate
# On Windows:
venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Jupyter Notebook Execution

```bash
# Start Jupyter Notebook
jupyter notebook

# Open notebook in browser and run all cells
# File: Tesla_Stock_Price_Prediction.ipynb
```

### 3. Streamlit Application

```bash
# Navigate to project directory
cd path/to/Tesla-Stock-Price-Prediction

# Run Streamlit app
streamlit run app.py

# App opens at http://localhost:8501
```

---

## Cloud Deployment Options

### Option 1: Deploy on Streamlit Cloud

#### Steps:
1. **Push to GitHub**
   ```bash
   git add .
   git commit -m "Ready for deployment"
   git push origin main
   ```

2. **Create Streamlit Cloud Account**
   - Go to [share.streamlit.io](https://share.streamlit.io)
   - Sign in with GitHub
   - Click "New app"

3. **Deploy Application**
   - Repository: Github-Emmi/Tesla-Stock-Price-Prediction
   - Branch: main
   - Main file path: app.py
   - Click "Deploy"

4. **Application URL**
   - Live at: `https://share.streamlit.io/[username]/Tesla-Stock-Price-Prediction/main/app.py`

### Option 2: Deploy on AWS

#### Using EC2 Instance:

```bash
# SSH into EC2 instance
ssh -i "key.pem" ubuntu@[ec2-instance-ip]

# Install Python and dependencies
sudo apt-get update
sudo apt-get install python3 python3-pip

# Clone repository
git clone https://github.com/Github-Emmi/Tesla-Stock-Price-Prediction.git
cd Tesla-Stock-Price-Prediction

# Install requirements
pip3 install -r requirements.txt

# Run Streamlit with public access
streamlit run app.py \
  --server.port=8501 \
  --server.address=0.0.0.0 \
  --server.enableCORS=false \
  --server.enableXsrfProtection=false
```

### Option 3: Deploy on Heroku

#### Heroku Deployment File:
Create `Procfile`:
```
web: streamlit run app.py
```

#### Deploy:
```bash
# Install Heroku CLI
# Create Heroku app
heroku create tesla-price-prediction

# Deploy
git push heroku main

# View logs
heroku logs --tail
```

---

## Production Considerations

### 1. Model Versioning

```
models/
├── v1.0/
│   ├── simplernn_model.h5
│   ├── lstm_model.h5
│   └── scaler.pkl
├── v1.1/
│   ├── simplernn_model.h5
│   ├── lstm_model.h5
│   └── scaler.pkl
└── v2.0/ (current)
    ├── simplernn_model.h5
    ├── lstm_model.h5
    └── scaler.pkl
```

### 2. Configuration Management

Create `config.yaml`:
```yaml
# Model Configuration
model:
  simplernn_units: [50, 25]
  lstm_units: [50, 25]
  dropout_rate: 0.2
  batch_size: 32
  epochs: 100

# Data Configuration
data:
  window_size: 60
  train_test_split: 0.8
  normalization: "minmax"

# Deployment
deployment:
  environment: "production"
  debug: false
  max_workers: 4
```

### 3. Logging & Monitoring

```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('app.log'),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)

# Log important events
logger.info("Application started")
logger.info(f"Model prediction: {prediction}")
logger.error(f"Error in data loading: {error}")
```

---

## Maintenance Procedures

### 1. Regular Model Retraining

**Schedule**: Monthly or when new data available

```bash
# Automated retraining script
python scripts/retrain_models.py

# Steps:
# 1. Load latest TSLA data
# 2. Retrain both models
# 3. Evaluate performance
# 4. Compare with previous model
# 5. If improved, update production model
# 6. Archive old model
```

### 2. Data Updates

**Schedule**: Weekly or as new data becomes available

```bash
# Download latest data
python scripts/update_data.py

# Steps:
# 1. Fetch latest stock data
# 2. Validate data quality
# 3. Append to existing dataset
# 4. Trigger model retraining if needed
```

### 3. Monitoring & Alerts

```python
# Monitor prediction accuracy on new data
def monitor_predictions(actual_prices, predicted_prices):
    mse = mean_squared_error(actual_prices, predicted_prices)
    rmse = sqrt(mse)
    mae = mean_absolute_error(actual_prices, predicted_prices)
    
    # Alert if metrics degrade
    if rmse > RMSE_THRESHOLD:
        send_alert(f"RMSE degraded to {rmse}")
    
    log_metrics(mse, rmse, mae)
```

### 4. Performance Tracking

Track these metrics:
- **MSE**: Monitor for degradation
- **RMSE**: Compare month-over-month
- **Prediction Latency**: Track inference time
- **System Uptime**: Monitor availability
- **User Engagement**: Track app usage

---

## Backup & Recovery

### 1. Backup Strategy

```bash
# Daily backups
backup_dir="backups/$(date +%Y-%m-%d)"
mkdir -p "$backup_dir"

# Backup models
cp models/v2.0/* "$backup_dir/"

# Backup data
cp TSLA.csv "$backup_dir/TSLA_$(date +%Y-%m-%d).csv"

# Backup to cloud (AWS S3)
aws s3 cp "$backup_dir" s3://tesla-predictions-backup/ --recursive
```

### 2. Recovery Procedures

```bash
# Restore from backup
restore_date="2026-01-10"
backup_dir="backups/$restore_date"

# Restore models
cp "$backup_dir"/*.h5 models/v2.0/
cp "$backup_dir"/*.pkl models/v2.0/

# Verify restoration
python scripts/verify_models.py
```

---

## Troubleshooting Guide

### Common Issues & Solutions

#### Issue 1: Model Takes Too Long to Train
```
Solution:
1. Check available system resources (RAM, CPU)
2. Reduce batch size
3. Use GPU acceleration (install CUDA)
4. Reduce sequence length
5. Use smaller subset of data for testing
```

#### Issue 2: Low Prediction Accuracy
```
Solution:
1. Check data quality (missing values, outliers)
2. Retrain model from scratch
3. Tune hyperparameters
4. Increase training epochs
5. Try different model architecture
```

#### Issue 3: Streamlit App Crashes
```
Solution:
1. Check error logs
2. Verify all dependencies installed
3. Check model file paths
4. Verify data file availability
5. Restart application
```

#### Issue 4: Predictions Out of Range
```
Solution:
1. Verify scaler is loaded correctly
2. Check inverse transformation
3. Validate input data normalization
4. Retrain model with fresh data
```

---

## Maintenance Schedule

| Task | Frequency | Owner | Status |
|------|-----------|-------|--------|
| Monitor Performance | Daily | DevOps | __ |
| Update Data | Weekly | Data Engineer | __ |
| Check Logs | Daily | DevOps | __ |
| Retrain Models | Monthly | ML Engineer | __ |
| Backup Data | Daily | DevOps | __ |
| Security Update | Quarterly | Security | __ |
| Performance Optimization | Quarterly | DevOps | __ |
| Documentation Update | As needed | Tech Writer | __ |

---

## Support & Escalation

### Contact Information

| Role | Contact | Availability |
|------|---------|--------------|
| ML Engineer | [email] | 9 AM - 5 PM EST |
| DevOps | [email] | On-call 24/7 |
| Data Engineer | [email] | 9 AM - 5 PM EST |

### Escalation Path

1. **Tier 1 Support**: Check logs and documentation
2. **Tier 2 Support**: Contact DevOps engineer
3. **Tier 3 Support**: Contact ML/Lead engineer
4. **Tier 4 Support**: Executive escalation if needed

---

## Future Improvements

### Planned Enhancements

- [ ] Add additional features (trading volume, sentiment analysis)
- [ ] Implement ensemble models
- [ ] Add attention mechanisms
- [ ] Implement real-time prediction updates
- [ ] Create mobile application
- [ ] Add portfolio optimization features
- [ ] Implement automated trading recommendations
- [ ] Create advanced analytics dashboard

### Technology Upgrades

- [ ] Upgrade to latest TensorFlow version
- [ ] Implement GPU optimization
- [ ] Add distributed training capability
- [ ] Migrate to cloud-native architecture

---

## Document Control

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-01-12 | Data Science Team | Initial deployment |
| | | | |

---

**Document Created**: June 11, 2026  
**Last Updated**: June 11, 2026  
**Version**: 1.0
