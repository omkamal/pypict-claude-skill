# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code skill for designing comprehensive test cases using PICT (Pairwise Independent Combinatorial Testing). The skill enables systematic test case design that reduces test cases by 80-99% while maintaining comprehensive coverage through pairwise combinatorial testing.

**Core Purpose**: Given requirements or code, analyze the system to identify test parameters, generate PICT models with appropriate constraints, and format results with expected outputs.

## Installation

Skills in Claude Code are installed by placing them in the skills directory:
- **Personal (all projects)**: `~/.claude/skills/pict-test-designer/`
- **Project-specific**: `.claude/skills/pict-test-designer/`

Installation command:
```bash
# Personal installation
git clone https://github.com/omkamal/pypict-claude-skill.git ~/.claude/skills/pict-test-designer

# Project-specific installation
git clone https://github.com/omkamal/pypict-claude-skill.git .claude/skills/pict-test-designer
```

## Repository Architecture

### Skill Definition (`SKILL.md`)
The main skill definition that Claude Code loads. Contains:
- Complete workflow for test case design (5 steps: analyze → generate model → execute → determine outputs → format)
- Parameter identification guidelines using equivalence partitioning
- PICT model generation patterns
- Constraint writing best practices
- Expected output determination methods

### Documentation Structure
- **README.md**: Installation guide with correct manual installation steps
- **QUICKSTART.md**: 5-minute getting started guide with examples
- **STRUCTURE.md**: Complete repository structure and file descriptions
- **CONTRIBUTING.md**: Contribution guidelines, file structure, commit conventions

### Examples Directory (`examples/`)
Real-world PICT testing examples following this structure:
- `specification.md`: Original requirements/specification
- `test-plan.md`: Complete test plan with PICT model and generated test cases
- **ATM Example**: Comprehensive ATM system with 8 parameters, reducing 25,920 combinations to 31 test cases (99.88% reduction)

### Helper Scripts (`scripts/`)
Python utilities for PICT model manipulation:
- `pict_helper.py`: Generate models from JSON config, format output as markdown, parse PICT output to JSON


## Key Workflows

### Test Case Design Workflow (from SKILL.md)

1. **Analyze Requirements/Code**
   - Identify parameters (input variables, configuration options, environmental factors)
   - Determine values using equivalence partitioning and boundary values
   - Extract constraints (business rules, technical limitations, dependencies)
   - Define expected outcomes

2. **Generate PICT Model**
   ```
   # Parameters
   ParameterName: Value1, Value2, Value3

   # Constraints
   IF [Parameter1] = "Value" THEN [Parameter2] <> "OtherValue";
   ```

3. **Execute Model**
   - User saves model to file
   - Uses online tools (pairwise.yuuniworks.com, pairwise.teremokgames.com)
   - Or installs PICT locally

4. **Determine Expected Outputs**
   - Based on business requirements and code logic
   - Specific descriptions (not vague "success" or "error")

5. **Format Complete Test Suite**
   - PICT model with parameters and constraints
   - Markdown table with test cases and expected outputs
   - Summary with coverage metrics

### Parameter Identification Best Practices

**Good Practices:**
- Descriptive names: `AuthMethod`, `UserRole`, `PaymentType`
- Equivalence partitioning: `FileSize: Small, Medium, Large`
- Boundary values: `Age: 0, 17, 18, 65, 66`
- Negative testing: `Amount: ~-1, 0, 100, ~999999`

**Avoid:**
- Generic names: `Param1`, `Value1`
- Too many values without partitioning
- Missing edge cases

### Constraint Writing

**Good:**
- Document rationale: `# Safari only available on MacOS`
- Start simple, add incrementally
- Test constraints work as expected

**Avoid:**
- Over-constraining (eliminates too many valid combinations)
- Under-constraining (generates invalid test cases)

## Common Testing Patterns

### Web Form Testing
```python
parameters = {
    "Name": ["Valid", "Empty", "TooLong"],
    "Email": ["Valid", "Invalid", "Empty"],
    "Password": ["Strong", "Weak", "Empty"],
    "Terms": ["Accepted", "NotAccepted"]
}
```

### API Endpoint Testing
```python
parameters = {
    "HTTPMethod": ["GET", "POST", "PUT", "DELETE"],
    "Authentication": ["Valid", "Invalid", "Missing"],
    "ContentType": ["JSON", "XML", "FormData"],
    "PayloadSize": ["Empty", "Small", "Large"]
}
```

### Configuration Testing
```python
parameters = {
    "Environment": ["Dev", "Staging", "Production"],
    "CacheEnabled": ["True", "False"],
    "LogLevel": ["Debug", "Info", "Error"],
    "Database": ["SQLite", "PostgreSQL", "MySQL"]
}
```

## Python Helper Script Usage

The `scripts/pict_helper.py` script provides utilities but is currently a basic implementation. For most use cases, generate PICT models directly in Python code as shown in SKILL.md workflow step 3.

```bash
# Generate PICT model from JSON config
python scripts/pict_helper.py generate config.json > model.txt

# Format PICT output as markdown table
python scripts/pict_helper.py format output.txt

# Parse PICT output to JSON
python scripts/pict_helper.py parse output.txt
```

**Config file structure:**
```json
{
    "parameters": {
        "Browser": ["Chrome", "Firefox", "Safari"],
        "OS": ["Windows", "MacOS", "Linux"]
    },
    "constraints": [
        "IF [OS] = \"MacOS\" THEN [Browser] <> \"IE\""
    ]
}
```

**Note**: The helper script does not execute PICT itself. Users must use online tools or install PICT locally to generate actual test cases from the model.

## PICT Syntax Reference

### Basic Structure
- Parameters: `ParameterName: Value1, Value2, Value3`
- Constraints: `IF [Param1] = "Value" THEN [Param2] <> "OtherValue";`
- Comments: `# This is a comment`

### Constraint Operators
- `=` Equal to
- `<>` Not equal to
- `>`, `<`, `>=`, `<=` Comparison
- `IN {Value1, Value2}` Set membership
- `NOT` Logical negation
- `AND`, `OR` Logical operators
- `THEN` Implication
- `~` Negative testing prefix

### Advanced Features
- Sub-models for different coverage orders
- Aliasing for value sets
- Negative testing with `~` prefix
- Weighted parameters for test prioritization

## Output Format

Present results in this structure:

````markdown
## PICT Model

```
# Parameters
Parameter1: Value1, Value2
Parameter2: ValueA, ValueB

# Constraints
IF [Parameter1] = "Value1" THEN [Parameter2] = "ValueA";
```

## Generated Test Cases

| Test # | Parameter1 | Parameter2 | Expected Output |
| --- | --- | --- | --- |
| 1 | Value1 | ValueA | Success: ... |
| 2 | Value2 | ValueB | Error: ... |

## Test Case Summary

- Total test cases: N
- Coverage: Pairwise (all 2-way combinations)
- Constraints applied: N
- Reduction: X% from exhaustive testing
````

## Working with Examples

### Existing Examples
- **ATM System** (`examples/atm-*`): Comprehensive financial system example
  - Shows reduction from 25,920 combinations to 31 test cases (99.88%)
  - Demonstrates complex constraints with 8 parameters
  - Includes detailed specification and complete test plan

### Adding New Examples

When contributing examples to `examples/`:

1. Follow the flat file structure in `examples/` (not subdirectories):
   - `example-name-specification.md`: Original requirements
   - `example-name-test-plan.md`: Complete test plan with PICT model and test cases
2. Update `examples/README.md` to list the new example
3. See CONTRIBUTING.md for complete guidelines

## Key Principles

- **Equivalence Partitioning**: Group similar values to reduce parameter space
- **Boundary Value Analysis**: Include edge cases (0, 1, max-1, max)
- **Negative Testing**: Use `~` prefix to test invalid values
- **Pairwise Coverage**: Reduces test cases by 80-99% vs exhaustive testing
- **Constraint Documentation**: Always explain the rationale for constraints
- **Specific Expectations**: Expected outputs should be detailed, not vague

## Important Implementation Notes

### Skill Activation
This skill is defined in `SKILL.md` and is loaded by Claude Code when:
- User requests test case design
- User mentions PICT, pairwise testing, or combinatorial testing
- User asks about testing with multiple parameters

### PICT Execution
**Critical**: This skill generates PICT models but does NOT execute PICT itself. Users must:
1. Save the generated model to a `.txt` file
2. Use online tools (pairwise.yuuniworks.com, pairwise.teremokgames.com) OR
3. Install PICT locally and run: `pict model.txt > output.txt`

### Direct Python Generation
For most use cases, generate PICT models directly in Python as shown in SKILL.md step 3, rather than relying on the helper script.

## Dependencies

- Python 3.7+ (for helper scripts only, not required for main skill)
- pypict (optional, for direct PICT integration): `pip install pypict --break-system-packages`
- Online PICT tools available at:
  - https://pairwise.yuuniworks.com/
  - https://pairwise.teremokgames.com/

## Credits

Built upon:
- **Microsoft PICT**: Original combinatorial testing tool
- **pypict**: Python binding by Kenichi Maehashi
