# Fluvius Ordinal

Fluvius Ordinal is a rule processing and knowledge engine library.

## Overview

This package contains rule processing and knowledge engine functionality originally from the rulepy package, adapted to work as an independent Python package called `fluvius_ordinal`.

## Features

- **Rule Engine**: Define and execute business rules using Python decorators
- **Knowledge Base**: Store and manage facts and rules in a knowledge base
- **Working Memory**: Efficient fact storage and retrieval system
- **Rule Decorators**: Easy-to-use decorators for defining rules with conditions
- **Pattern Matching**: Advanced pattern matching for rule conditions
- **Forward Chaining**: Automatic inference engine for rule execution
- **Expert System**: Build expert systems with rule-based reasoning

## Installation

```bash
pip install fluvius-ordinal
```

For development:

```bash
pip install fluvius-ordinal[dev]
```

For testing:

```bash
pip install fluvius-ordinal[test]
```

For documentation:

```bash
pip install fluvius-ordinal[docs]
```

## Quick Start

### Basic Rule Definition

```python
from fluvius_ordinal import KnowledgeEngine, rule, when
from fluvius_ordinal.datadef import RuleNarration

class MyRuleEngine(KnowledgeEngine):
    
    @rule(when(lambda x: x > 10))
    def large_number_rule(self, x):
        """Rule for handling large numbers."""
        print(f"Found a large number: {x}")
        return RuleNarration(
            description=f"Number {x} is large",
            action="logged_large_number"
        )
    
    @rule(when(lambda x: x % 2 == 0))
    def even_number_rule(self, x):
        """Rule for handling even numbers."""
        print(f"Found an even number: {x}")
        return RuleNarration(
            description=f"Number {x} is even",
            action="logged_even_number"
        )

# Usage
engine = MyRuleEngine()
engine.declare(15)  # Triggers large_number_rule
engine.declare(8)   # Triggers both large_number_rule and even_number_rule
engine.run()
```

### Knowledge Base Usage

```python
from fluvius_ordinal import KnowledgeBase, WorkingMemory

# Create a knowledge base
kb = KnowledgeBase()

# Add facts
kb.add_fact("temperature", 75)
kb.add_fact("humidity", 60)
kb.add_fact("season", "summer")

# Create working memory
wm = WorkingMemory()
wm.load_from_knowledge_base(kb)

# Query facts
temp = wm.get_fact("temperature")
print(f"Current temperature: {temp}")
```

### Advanced Rule Patterns

```python
from fluvius_ordinal import KnowledgeEngine, rule, when

class WeatherRuleEngine(KnowledgeEngine):
    
    @rule(
        when(lambda temp: temp > 80),
        when(lambda humidity: humidity > 70)
    )
    def hot_humid_rule(self, temp, humidity):
        """Rule for hot and humid weather."""
        return RuleNarration(
            description="Hot and humid weather detected",
            action="suggest_air_conditioning",
            data={"temperature": temp, "humidity": humidity}
        )
    
    @rule(when(lambda season: season == "winter"))
    def winter_rule(self, season):
        """Rule for winter season."""
        return RuleNarration(
            description="Winter season detected",
            action="suggest_heating",
            data={"season": season}
        )

# Usage
engine = WeatherRuleEngine()
engine.declare_fact("temperature", 85)
engine.declare_fact("humidity", 75)
engine.declare_fact("season", "summer")
engine.run()
```

## Architecture

The package follows a modular architecture:

- **Engine**: Core rule processing engine with forward chaining
- **Knowledge Base**: Fact storage and management system
- **Working Memory**: Efficient in-memory fact storage
- **Decorators**: Easy rule definition using Python decorators
- **Data Definitions**: Structured data models for rules and facts

## Rule Definition Concepts

### Rules
Rules are defined using the `@rule` decorator and consist of:
- **Conditions**: When clauses that specify when the rule should fire
- **Actions**: The code executed when conditions are met
- **Priority**: Optional priority for rule execution order

### Facts
Facts are pieces of information stored in the knowledge base:
- Can be any Python object
- Indexed for efficient retrieval
- Support pattern matching

### Working Memory
The working memory is where facts are stored during rule execution:
- Efficient storage and retrieval
- Automatic conflict resolution
- Support for fact retraction

## Migration from Rulepy

If you're migrating from the original rulepy package:

1. Update imports from `fluvius.rulepy` to `fluvius_ordinal`
2. Update any configuration references
3. Test your rule definitions and knowledge bases

## Requirements

- Python 3.12+
- Pydantic (for data validation)
- PyRsistent (for immutable data structures)

## Development

### Setting up for Development

```bash
git clone https://github.com/adaptive-bits/fluvius-ordinal.git
cd fluvius-ordinal
pip install -e .[dev]
```

### Running Tests

```bash
pytest
```

### Code Formatting

```bash
black src tests
ruff check src tests
```

### Building Documentation

```bash
cd docs
make html
```

## Examples

See the `examples/` directory for more comprehensive examples:
- Basic rule engine setup
- Advanced pattern matching
- Expert system implementation
- Integration with external data sources

## Performance

The rule engine is optimized for:
- Fast rule matching using efficient algorithms
- Minimal memory usage with pyrsistent data structures
- Scalable fact storage and retrieval

## License

MIT License - see LICENSE file for details.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Run the test suite
6. Submit a pull request

## Support

For issues and questions, please use the GitHub issue tracker.

## Acknowledgments

This package is derived from the original rulepy module in the Fluvius framework.
