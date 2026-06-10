# Tesla V-Shape GitHub Agent - Quick Reference Guide

## 🎯 What is This Agent?

A specialized GitHub coordinator that manages your entire Tesla Stock Price Prediction project across 10 V-Shape SDLC phases. It automates GitHub operations, enforces quality standards, and keeps everything aligned with your project documentation.

---

## 🚀 Quick Start

### Invoke the Agent
Simply mention it in your request:

```
@tesla-vshape-github-agent [Your task]
```

Or directly describe what you need:

```
"Create a GitHub PR to implement the data pipeline module"
```

The agent will automatically load and handle it.

---

## 📋 Common Tasks

### Task 1: Implement a Module (Phase 5)

**Your Request**:
```
"Create a GitHub PR to implement data_pipeline.py with all required functions:
- load_data(filepath)
- validate_data(df)
- handle_missing_values(df)
- normalize_data(df)
- create_sequences(data, window_size)
- split_train_test(X, y, test_size)

Include full docstrings, error handling, and PEP 8 compliance."
```

**Agent Will**:
1. ✅ Create feature branch: `feature/phase-5.1-data-pipeline`
2. ✅ Assign Copilot to implement all functions
3. ✅ Link PR to requirements FR1-FR4
4. ✅ Validate PEP 8 compliance
5. ✅ Ensure docstrings on all functions
6. ✅ Run tests and check coverage
7. ✅ Auto-merge when all checks pass

**Time to Completion**: 20-30 minutes

---

### Task 2: Train Models (Phase 5.2)

**Your Request**:
```
"Implement Phase 5.2: Build and train SimpleRNN and LSTM models with:
- Model builders for both architectures
- GridSearchCV hyperparameter tuning
- Early stopping and model checkpointing
- Unit tests ensuring MSE < 100

Follow ARCHITECTURE_DESIGN.md specifications."
```

**Agent Will**:
1. ✅ Create branch: `feature/phase-5.2-model-trainer`
2. ✅ Delegate to Copilot with full spec from ARCHITECTURE_DESIGN.md
3. ✅ Link to FR5-FR10 requirements
4. ✅ Validate model performance (MSE < 100)
5. ✅ Run integration tests with evaluation module
6. ✅ Track model artifacts

**Time to Completion**: 45-60 minutes

---

### Task 3: Add Unit Tests (Phase 6)

**Your Request**:
```
"Create comprehensive unit tests for all modules following TEST_PLAN.md:
- 26+ test cases
- 90%+ code coverage
- pytest framework
- Include data pipeline, model trainer, and evaluation tests"
```

**Agent Will**:
1. ✅ Create branch: `test/phase-6-unit-tests`
2. ✅ Analyze coverage gaps
3. ✅ Delegate test implementation
4. ✅ Generate coverage report
5. ✅ Ensure 90%+ coverage
6. ✅ Link to Phase 6 milestone
7. ✅ Auto-merge with badge

**Time to Completion**: 1-2 hours

---

### Task 4: Deploy Streamlit App (Phase 10)

**Your Request**:
```
"Create deployment branch for Streamlit application following DEPLOYMENT_GUIDE.md:
- Streamlit Cloud configuration
- Environment setup
- Model loading from artifacts
- Performance monitoring
- Production readiness checklist"
```

**Agent Will**:
1. ✅ Create branch: `deploy/phase-10-streamlit`
2. ✅ Generate deployment checklist
3. ✅ Validate all tests pass
4. ✅ Create release notes
5. ✅ Link to Milestone 4
6. ✅ Coordinate production deployment

**Time to Completion**: 30-45 minutes

---

## 🔍 What the Agent Knows

### Project Structure
```
Phase 1: Planning & Requirements
Phase 2: System Design
Phase 3: Architecture Design
Phase 4: Module Design
Phase 5: Implementation
Phase 6: Unit Testing
Phase 7: Integration Testing
Phase 8: System Testing
Phase 9: Acceptance Testing
Phase 10: Deployment & Maintenance
```

### Modules
- **Data Pipeline**: Load, validate, preprocess data
- **Model Trainer**: Build SimpleRNN & LSTM, hyperparameter tuning
- **Evaluation**: Calculate metrics, compare models
- **Streamlit UI**: Interactive dashboard and predictions

### Requirements
- **22 Functional Requirements** (FR1-FR22)
- **11 Non-Functional Requirements** (NFR1-NFR11)
- **Success Criteria**: MSE < 100, Accuracy > 85%, Code Quality > 90%

### Quality Standards
- PEP 8 compliance
- 90%+ test coverage
- Docstrings on all functions
- Error handling with try-except
- Clear inline comments
- Linked to requirements

---

## ✅ Quality Checks

The agent automatically checks:

- ✅ **Code Quality**: PEP 8, flake8, mypy
- ✅ **Coverage**: 90%+ coverage required
- ✅ **Tests**: All tests pass
- ✅ **Documentation**: Docstrings, comments
- ✅ **Requirements**: Linked to FR/NFR
- ✅ **Phase Alignment**: Correct phase and module
- ✅ **Security**: No hardcoded secrets

---

## 📊 Tracking Progress

The agent tracks:

| Metric | Target | Tracking |
|--------|--------|----------|
| MSE | < 100 | Model performance |
| Accuracy | > 85% | Test metrics |
| Code Quality | > 90% | Coverage reports |
| Documentation | 100% | Checklist |
| On-Time | ✅ | Milestone dates |

---

## 🔑 Key Commands

### Create Feature Branch
```
"Create a feature branch for [Phase X] - [Module Name]"
```

### Assign Implementation Task
```
"Implement [Function/Module] from [Documentation File]"
```

### Run Tests
```
"Run tests for [Module] and ensure [Coverage/Performance]"
```

### Create Release
```
"Create a release for [Milestone] with version [X.Y.Z]"
```

### Generate Report
```
"Generate [Phase] completion report"
```

---

## 🎓 Learning Path

### For New Team Members
1. Start with `.github/AGENTS.md` (this file)
2. Read `VSHAPE_SDLC_OVERVIEW.md` (project overview)
3. Review `Documentations/01-Planning_Requirements/REQUIREMENTS.md`
4. Use agent to implement Phase 5 modules one-by-one
5. Refer to phase documentation as needed

### For Experienced Developers
1. Review specific phase documentation
2. Use agent to create PR with full specifications
3. Follow quality gates automatically
4. Merge when all checks pass

---

## 🐛 Troubleshooting

### Agent Not Responding?
- Check GitHub CLI is installed: `gh --version`
- Verify authentication: `gh auth login`
- Check internet connection
- Restart VS Code

### PR Merge Failing?
- Check test coverage: `pytest --cov`
- Verify PEP 8: `flake8 src/`
- Ensure docstrings present
- Check requirement links

### Model Performance Not Meeting Goals?
- Review ARCHITECTURE_DESIGN.md for specs
- Check hyperparameter tuning (GridSearchCV)
- Validate data preprocessing
- Increase training epochs
- Contact ML engineer

---

## 📞 Support

### Resources
- **Agent Documentation**: `.github/agents/tesla-vshape-agent.md`
- **Project Overview**: `VSHAPE_SDLC_OVERVIEW.md`
- **Phase Documentation**: `Documentations/` folder
- **Requirements**: See REQUIREMENTS.md
- **Implementation Guide**: See IMPLEMENTATION_GUIDE.md

### Contact
- For GitHub workflow issues: Use this agent
- For code issues: Use default agent
- For project questions: Refer to documentation

---

## 🎯 Next Steps

1. **Read** `VSHAPE_SDLC_OVERVIEW.md`
2. **Review** requirements in `Documentations/01-Planning_Requirements/`
3. **Use Agent** to implement Phase 5 modules
4. **Follow** quality gates
5. **Track** progress on GitHub

---

**Happy Coding! 🚀**

*Questions? Use the agent or refer to project documentation.*

Last Updated: June 11, 2026
