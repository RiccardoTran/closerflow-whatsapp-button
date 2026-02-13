# Configurable "Powered By" Section - Design Document

**Date:** 2026-02-13
**Status:** Approved
**Author:** Claude (with user approval)

## Overview

Make the "Powered By" footer section in the WhatsApp widget configurable, allowing users to show/hide it and customize the brand name and URL.

## Requirements

- Add configuration option to show/hide the "Powered By" section
- Make the brand name configurable
- Make the brand URL configurable
- Keep "⚡ Powered by" prefix fixed (not configurable)
- Default behavior: visible with "Trietlabs" branding
- Maintain backward compatibility

## Current Implementation

The "Powered By" section is currently hardcoded in `widget.js` at line 279-281:

```html
<div class="mv-powered-by">
    ⚡ Powered by <a href="https://www.closerflow.com" target="_blank" class="mv-closerflow-link">CloserFlow</a>
</div>
```

## Proposed Solution

### Approach: Conditional Rendering

Use template literals with conditional rendering to show/hide the section based on configuration.

**Rationale:**
- Clean and performant (no unnecessary DOM elements when hidden)
- Maintains template-based approach consistent with rest of widget
- Simple implementation with minimal code changes

## Configuration Schema

Add three new properties to `defaultConfig` in `embed.js`:

```javascript
const defaultConfig = {
    // ... existing properties ...
    showPoweredBy: true,              // Show/hide the powered by section
    poweredByBrand: 'Trietlabs',      // Brand name to display
    poweredByUrl: 'https://www.trietlabs.com'  // Brand URL
};
```

### Configuration Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `showPoweredBy` | boolean | `true` | Controls visibility of the powered by section |
| `poweredByBrand` | string | `'Trietlabs'` | Brand name displayed in the link |
| `poweredByUrl` | string | `'https://www.trietlabs.com'` | URL for the brand link |

### Usage Examples

**Hide the section completely:**
```javascript
window._whatsappConfig = {
    showPoweredBy: false
};
```

**Customize brand and URL:**
```javascript
window._whatsappConfig = {
    showPoweredBy: true,
    poweredByBrand: 'My Company',
    poweredByUrl: 'https://mycompany.com'
};
```

**Use defaults (no configuration needed):**
```javascript
// No config needed - will show "⚡ Powered by Trietlabs"
```

## Implementation Details

### File: `embed.js`

Add new properties to `defaultConfig` object (lines 41-54):

```javascript
const defaultConfig = {
    buttonName: 'WhatsApp',
    buttonIconSize: '24',
    brandImageUrl: '',
    brandName: '',
    brandSubtitleText: 'Typically replies within seconds',
    buttonSize: 'medium',
    buttonPosition: 'right',
    callToAction: 'Start Chat',
    phoneNumber: '',
    welcomeMessage: 'Hi there 👋',
    prefillMessage: 'Hi, I want to more about the program!',
    baseUrl: 'https://cdn.jsdelivr.net/gh/RiccardoTran/closerflow-whatsapp-widget@main',
    showPoweredBy: true,
    poweredByBrand: 'Trietlabs',
    poweredByUrl: 'https://www.trietlabs.com'
};
```

### File: `widget.js`

Replace hardcoded HTML at line 279-281 with conditional rendering:

**Before:**
```javascript
<div class="mv-powered-by">
    ⚡ Powered by <a href="https://www.closerflow.com" target="_blank" class="mv-closerflow-link">CloserFlow</a>
</div>
```

**After:**
```javascript
${config.showPoweredBy ? `
    <div class="mv-powered-by">
        ⚡ Powered by <a href="${config.poweredByUrl}" target="_blank" class="mv-closerflow-link">${config.poweredByBrand}</a>
    </div>
` : ''}
```

**Rendering Logic:**
- If `showPoweredBy` is `false` → empty string (no DOM element)
- If `showPoweredBy` is `true` → render div with configured brand and URL
- Emoji `⚡` and text "Powered by" remain fixed

### CSS Styles

No changes needed. Existing styles for `.mv-powered-by` and `.mv-closerflow-link` (lines 186-200) remain unchanged.

## Backward Compatibility

- Existing implementations without new config properties will use defaults
- Default `showPoweredBy: true` ensures section remains visible
- Default brand changes from "CloserFlow" to "Trietlabs"
- No breaking changes to API or behavior

## Data Flow

1. User sets `window._whatsappConfig` (optional)
2. `embed.js` merges user config with `defaultConfig`
3. Config passed to `initWhatsAppWidget(shadowRoot, config)`
4. `createWidget(config)` renders template with conditional powered by section
5. Template inserted into Shadow DOM

## Testing Strategy

Test with `test.html` using these scenarios:

### Test Case 1: Default Behavior
```javascript
// No powered by config specified
window._whatsappConfig = {
    phoneNumber: '1234567890',
    // ... other required fields
};
```
**Expected:** Shows "⚡ Powered by Trietlabs" linked to www.trietlabs.com

### Test Case 2: Hidden Section
```javascript
window._whatsappConfig = {
    showPoweredBy: false,
    phoneNumber: '1234567890'
};
```
**Expected:** No powered by section in DOM

### Test Case 3: Custom Brand
```javascript
window._whatsappConfig = {
    showPoweredBy: true,
    poweredByBrand: 'Custom Brand',
    poweredByUrl: 'https://custombrand.com',
    phoneNumber: '1234567890'
};
```
**Expected:** Shows "⚡ Powered by Custom Brand" linked to custombrand.com

### Test Case 4: Partial Config
```javascript
window._whatsappConfig = {
    poweredByBrand: 'OnlyBrand',
    phoneNumber: '1234567890'
};
```
**Expected:** Shows "⚡ Powered by OnlyBrand" linked to default URL (www.trietlabs.com)

## Edge Cases

- **Empty brand/URL:** If user sets `showPoweredBy: true` but provides empty strings, section renders but appears incomplete. This is user's responsibility.
- **Invalid URL:** Browser handles invalid URLs naturally. No additional validation needed.
- **XSS considerations:** Values are inserted directly into template. Since it's user's own configuration for their own site, this is expected behavior.

## Modifications Summary

| File | Lines | Change |
|------|-------|--------|
| `embed.js` | 41-54 | Add 3 properties to `defaultConfig` |
| `widget.js` | 279-281 | Replace hardcoded HTML with conditional template |

## Alternatives Considered

### CSS Display Toggle
- Always render element, use `style="display: none"` when hidden
- **Rejected:** Less clean, leaves unnecessary DOM element

### Dynamic DOM Manipulation
- Create element with `document.createElement()` and append if needed
- **Rejected:** More complex, breaks template-based pattern

## Success Criteria

- ✅ Users can hide powered by section with `showPoweredBy: false`
- ✅ Users can customize brand name and URL
- ✅ Default shows "Trietlabs" branding
- ✅ Existing implementations continue working
- ✅ No breaking changes
- ✅ Clean, maintainable code

## Next Steps

1. Implement changes in `embed.js` and `widget.js`
2. Test all scenarios in `test.html`
3. Update documentation/examples if needed
4. Commit and deploy
