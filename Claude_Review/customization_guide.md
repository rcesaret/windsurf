# Windsurf AI Customization Guide

## Quick Start Checklist

### ✅ MUST CUSTOMIZE (Project-Specific)
These files MUST be edited for your specific project:

#### Core Context (High Priority)
- [ ] `.windsurf/context/architecture.md` - Your system design
- [ ] `.windsurf/context/directory_structure.md` - Your project layout  
- [ ] `.windsurf/context/product_requirement_docs.md` - Your project goals
- [ ] `.windsurf/context/technical.md` - Your tech stack
- [ ] `.windsurf/context/glossary.md` - Your domain terms
- [ ] `.windsurf/context/examples.md` - Your code standards

#### Project Configuration
- [ ] `.windsurf/README.md` - Update project name and description
- [ ] `.windsurf/rules/52-project-conventions.md` - Your specific conventions
- [ ] `.windsurf/config/mcp_config.json` - Your tool configurations

### 🔧 SHOULD CUSTOMIZE (Language/Framework-Specific)
Customize these based on your technology choices:

#### Language-Specific Rules
- [ ] `.windsurf/rules/20-coding-standards.md` - Update for your languages
- [ ] `.windsurf/instructions/template-coding-standards.md` - Detailed standards
- [ ] `.windsurf/templates/` - Add templates for your tech stack

#### Development Process
- [ ] `.windsurf/rules/30-testing-standards.md` - Your testing approach
- [ ] `.windsurf/rules/50-version-control.md` - Your Git workflow
- [ ] `.windsurf/workflows/` - Your CI/CD processes

### 📋 CAN CUSTOMIZE (Optional)
These provide additional value but aren't required:

#### Advanced Features
- [ ] `.windsurf/knowledge/` - Add domain-specific knowledge
- [ ] `.windsurf/hooks/` - Add custom automation
- [ ] Operational modes in `instructions/` - Customize AI behavior

### ⚠️ DO NOT MODIFY (Framework Files)
These files provide the core framework - only modify if you're an expert:

#### Core Framework
- `.windsurf/rules/01-meta-rules.md` - Core AI behavior
- `.windsurf/rules/02-memory-access.md` - Memory protocols
- `.windsurf/rules/03-workflow-activation.md` - Mode transitions
- `.windsurf/core/personas/` - Persona definitions
- `.windsurf/memories/templates/` - Memory templates

## Customization Workflow

### Step 1: Initial Setup (30 minutes)
1. Clone this template repository
2. Update `.windsurf/README.md` with your project name
3. Fill out the context files with your project information
4. Commit initial customizations

### Step 2: Technology Alignment (1-2 hours)
1. Update coding standards for your languages
2. Configure your preferred tools in MCP config
3. Add technology-specific templates
4. Test basic AI interactions

### Step 3: Process Integration (2-4 hours)
1. Align rules with your development process
2. Configure CI/CD workflows
3. Set up version control conventions
4. Train team on new AI-assisted workflow

### Step 4: Advanced Customization (Ongoing)
1. Add domain knowledge as needed
2. Create custom automation hooks
3. Refine rules based on usage patterns
4. Contribute improvements back to template

## Common Customization Patterns

### For Web Development
```yaml
# Key files to focus on
tech_stack:
  - Update: rules/20-coding-standards.md (JavaScript/TypeScript)
  - Add: templates/react-component.template
  - Configure: workflows/deploy-staging.yml
  - Update: context/technical.md (Node.js, React, etc.)
```

### For Python/Data Science
```yaml
# Key files to focus on  
tech_stack:
  - Update: rules/20-coding-standards.md (Python/PEP8)
  - Add: templates/python-class.template
  - Add: knowledge/data-science-patterns.md
  - Update: context/technical.md (pandas, sklearn, etc.)
```

### For Enterprise/Team Usage
```yaml
# Key files to focus on
enterprise:
  - Customize: rules/50-version-control.md (enterprise Git workflow)
  - Add: knowledge/compliance-requirements.md
  - Configure: multiple persona configurations
  - Set up: advanced memory management
```

## Validation Checklist

After customization, verify:
- [ ] AI can read your architecture correctly
- [ ] Code generation follows your standards  
- [ ] Task management works with your workflow
- [ ] All team members understand the system
- [ ] Integration with your tools is working

## Getting Help

- **Documentation Issues**: Check the proposals for detailed explanations
- **Customization Questions**: Review this guide and examples
- **Advanced Features**: Consult the CASCADE ADDITIONS proposal
- **Implementation Problems**: Check the rule validation and error logs
