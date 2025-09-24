# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Character Generator** web application that creates detailed character profiles for roleplaying scenarios. The system supports multiple world backgrounds (modern urban, ancient/fantasy, sci-fi, fantasy magic) and generates comprehensive character data in YAML format.

## Development Commands

This is a static HTML/CSS/JavaScript application with no build system or package management:

- **Serve locally**: Open `index.html` or `character-generator.html` directly in a web browser, or use any local web server (e.g., `python -m http.server`, Live Server extension)
- **No build step required**: All files are served directly
- **No dependencies**: Pure vanilla JavaScript, no external frameworks
- **No testing framework**: Manual testing in browser required

## Architecture Overview

### File Structure
```
/
├── index.html              # World selection landing page
├── character-generator.html # Main character generation interface  
├── js/
│   ├── options-config.js   # Character trait/option definitions
│   └── worlds-config.js    # World-specific theming and options
└── 部署计划.md             # Deployment planning document (Chinese)
```

### Core Architecture

**Multi-World Theming System**:
- `WORLDS_CONFIG` in `worlds-config.js` defines different world backgrounds (modern, ancient, scifi, fantasy)
- Each world has unique color schemes, decorations, and specialized option sets
- World selection on `index.html` passes `?world=<type>` parameter to main generator

**Character Option System**:
- `OPTIONS_CONFIG` in `options-config.js` defines base character traits and options
- Supports both multi-select and single-select option types
- Gender-specific filtering for certain traits (e.g., body types, face descriptions)
- Configurable random count ranges for each option category

**Data Generation Pipeline**:
1. User fills form fields (basic info, appearance, personality, etc.)
2. `collectFormData()` gathers all input including selected option buttons
3. Custom inputs are merged with pre-defined selections
4. Final data exports as YAML format via `js-yaml` library

**UI Interaction Patterns**:
- Collapsible sections for organizing large option sets
- Option buttons with selected state management
- Custom input fields as fallback for unlisted options
- Real-time validation (especially for required name field)
- Modal display for final YAML output

### Key Data Structures

**World Configuration**:
```javascript
WORLDS_CONFIG = {
  worldKey: {
    name: "Display Name",
    color: { primary, secondary, accent, background },
    // World-specific option overrides
    positiveTraits: { type: 'multi-select', options: [...] }
  }
}
```

**Character Options**:
```javascript
OPTIONS_CONFIG = {
  fieldName: {
    type: 'multi-select' | 'single-select',
    randomCount: { min, max },
    genderSpecific: boolean,
    options: [...] | { female: [...], male: [...], common: [...] }
  }
}
```

## Important Implementation Details

### Character Option Categories
- **Basic Info**: Name, age, gender, sexual orientation
- **Personality**: Positive/negative traits, love views, social style
- **Appearance**: Face, body, skin color, eye color, hair, clothing style, accessories
- **Other Settings**: Illnesses, character traits, body scent, sensitivity, sexual history, kinks, hobbies, sensitive spots
- **Experience**: Identity/occupation, relationship status

### Gender-Aware Option Filtering
- Many appearance options are filtered by biological gender selection
- Uses `GENDER_MAPPING` to determine which option sets to show
- Supports inclusive options for non-binary/fluid gender selections

### World-Specific Customizations
Each world background can override default options with themed alternatives:
- **Modern**: Tech/social media focused traits, contemporary professions
- **Ancient**: Traditional virtues/vices, classical aesthetics, historical occupations  
- **Sci-fi**: Futuristic concepts, space-age technology, alien characteristics
- **Fantasy**: Magical abilities, mythical creatures, medieval fantasy elements

### Extensibility Features
- `CharacterGeneratorAPI` global object provides functions to modify option groups
- `addOptionGroup()`, `updateOptionGroup()`, `removeOptionGroup()` for runtime customization
- Dynamic option grid updates when gender or world changes
- Modular configuration system supports easy content expansion

## Code Style Notes

- Pure vanilla JavaScript (ES6+), no frameworks
- Extensive use of CSS custom properties for theming
- Responsive design with mobile-first approach
- Semantic HTML structure with accessibility considerations
- Modular JavaScript with clear separation of concerns
- Chinese language UI with English code comments