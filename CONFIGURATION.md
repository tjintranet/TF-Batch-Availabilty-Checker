# Trim Size Validator Configuration Guide

## Table of Contents
1. [Configuration File Structure](#configuration-file-structure)
2. [Adding New Configurations](#adding-new-configurations)
3. [Validation Rules](#validation-rules)
4. [Troubleshooting](#troubleshooting)
5. [Best Practices](#best-practices)
6. [Examples](#examples)

## Configuration File Structure

The `config.js` file uses this basic structure:
```javascript
const CONFIG = {
    BINDING_MAP: {
        // Binding type mappings
    },
    PAPER_WEIGHTS: {
        // Paper weight configurations
    }
};
```

### BINDING_MAP
Maps alternative binding names to standard names:
```javascript
BINDING_MAP: {
    'Hardback': 'Cased',    // Alternative -> Standard
    'Paperback': 'Limp',    // Alternative -> Standard
    'Cased': 'Hardback',    // Standard -> Alternative
    'Limp': 'Paperback'     // Standard -> Alternative
}
```

### PAPER_WEIGHTS
Contains all valid configurations:
```javascript
PAPER_WEIGHTS: {
    80: {
        dimensions: new Set(['156x234', '174x246']),
        validColors: new Set(['1']),
        bindings: new Set(['Cased', 'Limp'])
    }
}
```

## Adding New Configurations

### Adding New Trim Sizes
1. Identify the paper weight section
2. Add to the dimensions Set:
```javascript
dimensions: new Set([
    // Existing dimensions
    '156x234',
    '174x246',
    // New dimensions
    '180x250',
    '190x260'
])
```

### Adding New Paper Weights
1. Add a new entry to PAPER_WEIGHTS:
```javascript
PAPER_WEIGHTS: {
    // Existing weights...
    100: {
        dimensions: new Set([
            '156x234',
            '174x246'
        ]),
        validColors: new Set(['1']),
        bindings: new Set(['Cased', 'Limp'])
    }
}
```

2. Test all combinations thoroughly

### Adding New Color Specifications
1. Locate the paper weight section
2. Modify or add to validColors:
```javascript
validColors: new Set(['1', '4', 'Scattercolor'])
```

### Adding New Binding Types
1. Update BINDING_MAP with new mappings:
```javascript
BINDING_MAP: {
    'Hardback': 'Cased',
    'Paperback': 'Limp',
    'Wire': 'WireBound',        // New binding
    'WireBound': 'Wire'         // Reverse mapping
}
```

2. Add to bindings Set in relevant paper weights:
```javascript
bindings: new Set(['Cased', 'Limp', 'WireBound'])
```

## Validation Rules

### Paper Weight - Color Validation
- Each paper weight must specify valid colors
- Colors are strictly enforced per paper weight
- Color values must match exactly (case-sensitive)

### Dimension Validation
- Format must be 'widthxheight'
- No spaces allowed
- Numbers only
- Must be exact matches

### Binding Validation
- Must match configured bindings exactly
- Case-sensitive
- Mapped values are checked first

## Troubleshooting

### Common Issues

1. **Dimension Not Recognized**
   - Check format exactly matches 'widthxheight'
   - Verify no spaces
   - Confirm dimension exists in config
   ```javascript
   // Correct
   '156x234'
   // Wrong
   '156 x 234'
   '156X234'
   ' 156x234'
   ```

2. **Color Validation Fails**
   - Check case sensitivity
   - Verify color is valid for paper weight
   - Check for hidden spaces
   ```javascript
   // Example fix
   validColors: new Set(['1', '4'])  // Use exact values
   ```

3. **Binding Type Issues**
   - Verify binding exists in BINDING_MAP
   - Check case sensitivity
   - Confirm binding is valid for paper weight
   ```javascript
   // Add both mappings
   'NewType': 'Standard',
   'Standard': 'NewType'
   ```

4. **Excel Import Issues**
   - Verify column names match exactly
   - Check for hidden spaces in data
   - Confirm numeric values are numbers in Excel

### Debug Tips
1. Check console for validation errors
2. Verify config loading in browser console:
```javascript
console.log(CONFIG.PAPER_WEIGHTS)
```
3. Test individual validations:
```javascript
const paperWeight = CONFIG.PAPER_WEIGHTS[80];
console.log([...paperWeight.dimensions]);
```

## Best Practices

### Configuration Management
1. Comment all special cases
2. Keep dimensions sorted numerically
3. Group related configurations together
4. Document validation rules inline

### Code Organization
```javascript
// Group related paper weights
PAPER_WEIGHTS: {
    // Standard weights
    80: { /* ... */ },
    90: { /* ... */ },
    
    // Special weights
    100: { /* ... */ }
}
```

### Testing New Configurations
1. Create test Excel file with new combinations
2. Verify error messages
3. Test edge cases
4. Check export functionality

### Maintenance
1. Regular backup of config.js
2. Document all changes
3. Version control configurations
4. Test after each update

## Examples

### Complete Configuration Example
```javascript
const CONFIG = {
    BINDING_MAP: {
        'Hardback': 'Cased',
        'Paperback': 'Limp',
        'Cased': 'Hardback',
        'Limp': 'Paperback'
    },
    PAPER_WEIGHTS: {
        80: {
            dimensions: new Set([
                '156x234',
                '174x246'
            ]),
            validColors: new Set(['1']),
            bindings: new Set(['Cased', 'Limp'])
        },
        90: {
            dimensions: new Set([
                '156x234',
                '174x246'
            ]),
            validColors: new Set(['4']),
            bindings: new Set(['Cased', 'Limp'])
        }
    }
};
```

### Adding New Specification Example
```javascript
// Adding new paper weight with specific requirements
PAPER_WEIGHTS: {
    // Existing configurations...
    
    // New specialty paper weight
    120: {
        dimensions: new Set([
            '156x234',
            '174x246',
            '180x250'  // Special size for this weight
        ]),
        validColors: new Set(['1', 'Special']),  // Multiple colors
        bindings: new Set(['Cased', 'Limp', 'Special'])  // Custom binding
    }
}
```

Remember to always test thoroughly after making any configuration changes and maintain documentation of all updates.