---
# GitHub Agents Registry for Tesla Stock Price Prediction
# This file registers all custom agents used in the project
---

# Registered GitHub Agents

## 1. Tesla V-Shape GitHub Agent

**Agent ID**: `tesla-vshape-github-agent`  
**Location**: `.github/agents/tesla-vshape-agent.md`  
**Status**: ✅ Active

### Purpose
Specialized GitHub operations coordinator for the Tesla Stock Price Prediction V-Shape SDLC project. Manages workflow across all 10 phases, enforces enterprise code quality, coordinates module development, and automates testing & deployment.

### When to Use
- Managing GitHub operations (PRs, issues, branches, releases)
- Coordinating cross-phase deliverables
- Enforcing enterprise code quality standards
- Automating documentation workflows
- Managing model training pipelines
- Orchestrating testing phases
- Coordinating deployment

### Key Capabilities
- ✅ Phase-aware development workflow (10 V-Shape phases)
- ✅ Requirement-driven issue management (22 FR + 11 NFR)
- ✅ Module-based code organization (4 core modules)
- ✅ Testing automation & quality gates (90%+ coverage)
- ✅ Deep learning model management (SimpleRNN & LSTM)
- ✅ Enterprise documentation coordination
- ✅ Milestone & release management
- ✅ Automated code review

### Example Prompts
```
"Create a GitHub PR to implement data_pipeline.py module following Phase 5.1 
specifications. Include docstrings, error handling, and PEP 8 compliance."

"Implement Phase 5.2 model training with SimpleRNN and LSTM, GridSearchCV 
tuning, and validation tests ensuring MSE < 100."

"Create unit tests for all modules with 90%+ coverage following TEST_PLAN.md"

"Deploy Streamlit application following DEPLOYMENT_GUIDE.md"
```

### Tool Configuration
**Enabled Tools**:
- `mcp_github_mcp_se_create_pull_request_with_copilot`
- `mcp_github_mcp_se_assign_copilot_to_issue`
- `mcp_github_mcp_se_get_copilot_job_status`
- `mcp_github_mcp_se_create_branch`
- `mcp_github_mcp_se_push_files`
- Code search and analysis tools

**Disabled Tools**:
- Terminal execution (use dedicated agents)
- Playground code execution

### Project Context
- **Project**: Tesla Stock Price Prediction
- **Methodology**: V-Shape SDLC (10 Phases)
- **Tech Stack**: Python, TensorFlow/Keras, Streamlit, GitHub Actions
- **Success Criteria**: MSE < 100, Accuracy > 85%, Code Quality > 90%

### Integration Points
- GitHub repository operations
- Copilot task delegation
- GitHub Actions CI/CD
- Project documentation
- Testing frameworks (pytest)
- Model artifact management

---

## 2. Future Agent: ML Pipeline Agent

**Agent ID**: `ml-pipeline-agent` (Planned)  
**Purpose**: Specialized for Kaggle model training, experiment tracking, and ML pipeline orchestration

---

## 3. Future Agent: Testing Agent

**Agent ID**: `testing-agent` (Planned)  
**Purpose**: Dedicated to test automation, coverage analysis, and quality validation

---

## 4. Future Agent: Documentation Agent

**Agent ID**: `documentation-agent` (Planned)  
**Purpose**: Content generation, API documentation, and architecture diagram maintenance

---

## 5. Future Agent: Deployment Agent

**Agent ID**: `deployment-agent` (Planned)  
**Purpose**: Production operations, monitoring, and infrastructure management

---

## How to Invoke Custom Agents

### Using the Agent in Chat
1. Mention the agent by name or capability
2. VS Code will automatically load the appropriate agent
3. Agent will handle GitHub operations with full context

### Example Invocation
```
@tesla-vshape-github-agent Create a PR for Phase 5.1 data pipeline implementation
```

### Agent Selection Logic
The system automatically selects the Tesla V-Shape agent when:
- Working on GitHub PR/issue operations
- Discussing V-Shape SDLC phases
- Coordinating module development
- Managing project milestones
- Discussing quality standards
- Planning deployment

---

## Agent Lifecycle

### Activation Events
- Agent loads when working on GitHub operations
- Agent activates on phase-related discussions
- Agent engages when managing requirements

### Context Preservation
- Agent maintains full project documentation context
- Agent remembers phase dependencies
- Agent tracks requirement traceability
- Agent maintains code quality standards

### Team Collaboration
- Multiple team members can use the same agent
- Agent tracks work across all phases
- Agent coordinates cross-module dependencies
- Agent enforces consistent quality standards

---

## Configuration & Customization

### To Add/Update an Agent
1. Create `.agent.md` file in `.github/agents/`
2. Include YAML frontmatter with name, description, tools
3. Update this AGENTS.md registry
4. Commit and push to main branch
5. Agents auto-load in VS Code

### To Disable an Agent
- Rename file (e.g., `.agent.md.bak`)
- Or set `enabled: false` in frontmatter
- Or remove from this registry

---

## Best Practices

### For Team
- Use Tesla V-Shape agent for all project GitHub work
- Reference phase documentation when requesting tasks
- Follow quality gates before PR merge
- Keep agent configuration synchronized

### For Phase Transitions
- Use agent to track milestone completion
- Ensure all Phase N deliverables before Phase N+1
- Generate phase transition reports
- Update project status

### For Code Review
- Let agent run automated checks first
- Request human review after quality gates pass
- Agent tracks review history
- Agent manages approval workflows

---

## Troubleshooting

### Agent Not Loading
- Check `.github/agents/` folder exists
- Verify AGENTS.md registry is present
- Restart VS Code
- Check agent frontmatter YAML syntax

### Agent Not Responding to Prompts
- Verify agent description keywords match your request
- Check tool configuration (enabled/disabled)
- Review project context in agent documentation
- Try more specific phase or module references

### GitHub Integration Issues
- Verify GitHub CLI is installed and authenticated
- Check GitHub API token permissions
- Ensure repository is initialized with git
- Run `gh auth login` to refresh authentication

---

## Support & Documentation

- **Agent Guide**: `.github/agents/tesla-vshape-agent.md`
- **Phase Documentation**: `Documentations/` folder
- **Requirements**: `Documentations/01-Planning_Requirements/REQUIREMENTS.md`
- **Implementation**: `Documentations/05-Implementation/IMPLEMENTATION_GUIDE.md`
- **Testing**: `Documentations/06-Unit_Testing/TEST_PLAN.md`
- **Deployment**: `Documentations/10-Deployment_Maintenance/DEPLOYMENT_GUIDE.md`

---

**Last Updated**: June 11, 2026  
**Status**: ✅ Production Ready  
**Maintained By**: Tesla Stock Price Prediction Team
