# Trim Size Validator Documentation

## Overview
The Trim Size Validator is a web-based tool for validating book production specifications. It checks combinations of:
- Trim sizes (width × height)
- Paper weight (GSM)
- Binding types
- Color specifications

## File Structure
```
trim-size-validator/
├── index.html              # Main application file
├── css/
│   └── styles.css         # Custom styles
├── js/
│   └── config.js          # Configuration file
├── examples/
│   └── template.xlsx      # Example Excel template
└── README.md              # This documentation
```

## Configuration

### Adding New Trim Sizes
To add new trim size combinations, modify `js/config.js`. The configuration uses the following structure:

```javascript
const CONFIG = {
    PAPER_WEIGHTS: {
        80: {  // Paper weight in GSM
            dimensions: new Set([
                '123x186',  // Format: 'widthxheight'
                // Add new dimensions here
            ]),
            validColors: new Set(['1']),
            bindings: new Set(['Cased', 'Limp'])
        }
    }
};
```

To add new trim sizes:
1. Open `js/config.js`
2. Locate the appropriate paper weight section
3. Add new dimensions to the dimensions Set
4. Format must be exactly 'widthxheight' (no spaces)

Example adding new sizes to 80gsm:
```javascript
80: {
    dimensions: new Set([
        '123x186',
        '156x234',
        '174x246',
        '170x240',  // New size
        '180x250'   // New size
    ]),
    validColors: Set(['1']),
    bindings: Set(['Cased', 'Limp'])
}
```

### Adding New Paper Weights
To add a new paper weight:

```javascript
PAPER_WEIGHTS: {
    // Existing weights...
    100: {  // New paper weight
        dimensions: new Set([
            '156x234',
            '174x246'
        ]),
        validColors: new Set(['1']),  // Specify valid colors
        bindings: new Set(['Cased', 'Limp'])
    }
}
```

### Color Specifications
Colors are strictly tied to paper weights. To modify color rules:

1. Find the paper weight section
2. Modify the validColors Set
```javascript
validColors: new Set(['1', '4']),  // Example allowing both colors
```

### Binding Types
Standard binding types are 'Cased' and 'Limp'. To add new binding types:

1. Update the BINDING_MAP
```javascript
BINDING_MAP: {
    'Hardback': 'Cased',
    'Paperback': 'Limp',
    'Spiral': 'SpiralBound',  // New binding type
    'SpiralBound': 'Spiral'   // Reverse mapping
}
```

2. Add to valid bindings for each paper weight
```javascript
bindings: new Set(['Cased', 'Limp', 'SpiralBound'])
```

## Excel Template Format
The validator expects Excel files with the following columns:

| Column      | Type   | Description          | Valid Values                |
|-------------|--------|----------------------|----------------------------|
| ISBN        | String | Book identifier      | Any valid ISBN            |
| Description | String | Book description     | Any text                  |
| Width       | Number | Width in millimeters | Must match config values  |
| Height      | Number | Height in millimeters| Must match config values  |
| gsm         | Number | Paper weight         | Must match config keys    |
| Binding     | String | Binding type         | Must match config bindings|
| Color       | String/Number | Color code    | Must match validColors    |

## Validation Rules

### Paper Weight - Color Correlation
Each paper weight has specific valid colors:
- 80gsm: Only color '1'
- 90gsm: Only color '4'

### Size Validation
- Dimensions must exactly match the configured sizes for the specified paper weight
- Format must be width × height (both in millimeters)
- Values can be provided as numbers or strings

### Error Handling
The validator checks for:
1. Invalid paper weights
2. Invalid color combinations
3. Invalid dimensions
4. Invalid binding types

## Export Functionality
The validation results can be downloaded as an Excel file containing:
- All input specifications
- Validation result (Valid/Invalid)
- Detailed validation message
- Timestamp in filename

## Testing New Configurations
After making changes to config.js:
1. Test with the example template
2. Test each new combination
3. Verify error messages
4. Check export functionality

## Best Practices
1. Keep dimensions sorted for readability
2. Document any special cases in config.js
3. Test thoroughly after configuration changes
4. Backup config.js before major changes
5. Validate all new combinations before deployment

## Common Issues
1. Ensure dimensions are formatted exactly (e.g., '156x234')
2. Check case sensitivity in binding types
3. Verify color codes match exactly
4. Confirm GSM values are numbers in Excel files