# Powered By Configuration Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Make the "Powered By" footer section configurable (show/hide, custom brand/URL) with Trietlabs as default.

**Architecture:** Add three configuration properties to defaultConfig, modify widget template to use conditional rendering based on showPoweredBy flag, and inject brand/URL from config.

**Tech Stack:** Vanilla JavaScript, Template Literals, Shadow DOM

---

## Task 1: Add Configuration Properties to embed.js

**Files:**
- Modify: `embed.js:41-54`

**Step 1: Add three new properties to defaultConfig**

In `embed.js`, locate the `defaultConfig` object (lines 41-54) and add three new properties at the end:

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

**Step 2: Verify the change**

Read the file to confirm the three properties are correctly added with proper syntax (comma placement, quotes).

**Step 3: Commit**

```bash
git add embed.js
git commit -m "feat: add powered by configuration properties

- Add showPoweredBy (default: true)
- Add poweredByBrand (default: 'Trietlabs')
- Add poweredByUrl (default: 'https://www.trietlabs.com')

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"
```

---

## Task 2: Update Widget Template for Conditional Rendering

**Files:**
- Modify: `widget.js:279-281`

**Step 1: Replace hardcoded powered by HTML with conditional template**

In `widget.js`, locate the hardcoded powered by section at lines 279-281:

**Find this:**
```javascript
                        <div class="mv-powered-by">
                            ⚡ Powered by <a href="https://www.closerflow.com" target="_blank" class="mv-closerflow-link">CloserFlow</a>
                        </div>
```

**Replace with this:**
```javascript
                        ${config.showPoweredBy ? `
                        <div class="mv-powered-by">
                            ⚡ Powered by <a href="${config.poweredByUrl}" target="_blank" class="mv-closerflow-link">${config.poweredByBrand}</a>
                        </div>
                        ` : ''}
```

**Important details:**
- Use template literal syntax with `${}`
- Conditional: `config.showPoweredBy ? ... : ''`
- Inject `config.poweredByUrl` in href attribute
- Inject `config.poweredByBrand` as link text
- Keep emoji `⚡` and "Powered by" prefix fixed
- Maintain proper indentation (starts at column 24)

**Step 2: Verify the change**

Read the modified section to confirm:
- Conditional syntax is correct
- Template literals are properly nested
- No syntax errors (matching quotes, backticks)
- Indentation matches surrounding code

**Step 3: Commit**

```bash
git add widget.js
git commit -m "feat: implement configurable powered by section

- Use conditional rendering based on showPoweredBy
- Inject poweredByBrand and poweredByUrl from config
- Keep emoji and 'Powered by' prefix fixed

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"
```

---

## Task 3: Manual Testing - Default Behavior

**Files:**
- Modify: `test.html:56-70` (temporarily for testing)

**Step 1: Remove powered by config from test.html**

In `test.html`, locate the `window._whatsappConfig` object (lines 56-70) and ensure NO powered by properties are set (test default behavior).

The config should look like:
```javascript
window._whatsappConfig = {
    buttonName: 'Message Us',
    buttonIconSize: 24,
    brandImageUrl: 'https://via.placeholder.com/100',
    brandName: 'CloserFlow Demo',
    brandSubtitleText: 'Typically replies within seconds',
    buttonSize: 'medium',
    buttonPosition: 'right',
    callToAction: 'Start Chat',
    phoneNumber: '1234567890',
    welcomeMessage: 'Hi there 👋',
    prefillMessage: 'Hi, I want to know more!',
    baseUrl: '.'
};
```

**Step 2: Start local server**

```bash
node server.js
```

Expected output: `Server running at http://localhost:3001/`

**Step 3: Test in browser**

1. Open: http://localhost:3001/
2. Click the WhatsApp widget button to open popup
3. Verify footer shows: "⚡ Powered by Trietlabs"
4. Hover over "Trietlabs" link - verify URL is `https://www.trietlabs.com`
5. Open browser DevTools > Elements
6. Inspect Shadow DOM and confirm `<div class="mv-powered-by">` exists

**Expected result:** Default "Trietlabs" branding visible with correct URL.

**Step 4: Stop server**

Press `Ctrl+C` in terminal.

---

## Task 4: Manual Testing - Hidden Powered By

**Files:**
- Modify: `test.html:56-70` (temporarily for testing)

**Step 1: Add showPoweredBy: false to test.html**

In `test.html`, modify `window._whatsappConfig` to include:

```javascript
window._whatsappConfig = {
    buttonName: 'Message Us',
    buttonIconSize: 24,
    brandImageUrl: 'https://via.placeholder.com/100',
    brandName: 'CloserFlow Demo',
    brandSubtitleText: 'Typically replies within seconds',
    buttonSize: 'medium',
    buttonPosition: 'right',
    callToAction: 'Start Chat',
    phoneNumber: '1234567890',
    welcomeMessage: 'Hi there 👋',
    prefillMessage: 'Hi, I want to know more!',
    baseUrl: '.',
    showPoweredBy: false
};
```

**Step 2: Start local server**

```bash
node server.js
```

**Step 3: Test in browser**

1. Refresh: http://localhost:3001/
2. Click the WhatsApp widget button to open popup
3. Verify footer does NOT show "Powered by" section
4. Open browser DevTools > Elements
5. Inspect Shadow DOM and confirm `<div class="mv-powered-by">` does NOT exist in DOM

**Expected result:** No powered by section rendered.

**Step 4: Stop server**

Press `Ctrl+C` in terminal.

---

## Task 5: Manual Testing - Custom Brand

**Files:**
- Modify: `test.html:56-70` (temporarily for testing)

**Step 1: Add custom brand config to test.html**

In `test.html`, modify `window._whatsappConfig` to include:

```javascript
window._whatsappConfig = {
    buttonName: 'Message Us',
    buttonIconSize: 24,
    brandImageUrl: 'https://via.placeholder.com/100',
    brandName: 'CloserFlow Demo',
    brandSubtitleText: 'Typically replies within seconds',
    buttonSize: 'medium',
    buttonPosition: 'right',
    callToAction: 'Start Chat',
    phoneNumber: '1234567890',
    welcomeMessage: 'Hi there 👋',
    prefillMessage: 'Hi, I want to know more!',
    baseUrl: '.',
    showPoweredBy: true,
    poweredByBrand: 'Custom Brand',
    poweredByUrl: 'https://example.com'
};
```

**Step 2: Start local server**

```bash
node server.js
```

**Step 3: Test in browser**

1. Refresh: http://localhost:3001/
2. Click the WhatsApp widget button to open popup
3. Verify footer shows: "⚡ Powered by Custom Brand"
4. Hover over "Custom Brand" link - verify URL is `https://example.com`
5. Open browser DevTools > Elements
6. Inspect Shadow DOM and confirm link text and href are correct

**Expected result:** Custom brand "Custom Brand" visible with correct URL.

**Step 4: Stop server**

Press `Ctrl+C` in terminal.

---

## Task 6: Reset test.html to Default State

**Files:**
- Modify: `test.html:56-70`

**Step 1: Restore test.html to original config**

In `test.html`, restore `window._whatsappConfig` to default test configuration (no powered by overrides):

```javascript
window._whatsappConfig = {
    buttonName: 'Message Us',
    buttonIconSize: 24,
    brandImageUrl: 'https://via.placeholder.com/100',
    brandName: 'CloserFlow Demo',
    brandSubtitleText: 'Typically replies within seconds',
    buttonSize: 'medium',
    buttonPosition: 'right',
    callToAction: 'Start Chat',
    phoneNumber: '1234567890',
    welcomeMessage: 'Hi there 👋',
    prefillMessage: 'Hi, I want to know more!',
    baseUrl: '.'
};
```

**Step 2: Verify git status**

```bash
git status
```

Expected: `test.html` may or may not show as modified depending on whether you made temporary changes.

**Step 3: Discard test.html changes if modified**

```bash
git restore test.html
```

**Step 4: Verify clean state**

```bash
git status
```

Expected output: No modified files (or only untracked files like docs/plans).

---

## Task 7: Final Verification and Summary

**Step 1: Review all commits**

```bash
git log --oneline -3
```

Expected: Should see 2 commits:
1. `feat: implement configurable powered by section`
2. `feat: add powered by configuration properties`

**Step 2: Verify changes in both files**

Read both files to confirm:
- `embed.js` has 3 new properties in defaultConfig
- `widget.js` has conditional rendering for powered by section

**Step 3: Final smoke test**

```bash
node server.js
```

Visit http://localhost:3001/, open widget, verify "Trietlabs" branding appears.

**Step 4: Stop server**

Press `Ctrl+C`.

**Step 5: Push changes (if ready)**

```bash
git push origin main
```

---

## Success Criteria Checklist

- [ ] `embed.js` has `showPoweredBy: true` property
- [ ] `embed.js` has `poweredByBrand: 'Trietlabs'` property
- [ ] `embed.js` has `poweredByUrl: 'https://www.trietlabs.com'` property
- [ ] `widget.js` uses conditional rendering `${config.showPoweredBy ? ... : ''}`
- [ ] Template injects `config.poweredByBrand` and `config.poweredByUrl`
- [ ] Default behavior: shows "⚡ Powered by Trietlabs"
- [ ] With `showPoweredBy: false`: section not in DOM
- [ ] With custom brand/URL: shows custom values
- [ ] All changes committed with proper messages
- [ ] No breaking changes to existing implementations

---

## Edge Cases & Notes

**Edge Case 1: Empty brand or URL**
- If user sets `showPoweredBy: true` but provides empty strings for brand/URL
- Result: Section renders but appears incomplete
- This is user's responsibility - no validation added

**Edge Case 2: Invalid URL**
- Browser handles invalid URLs naturally
- No additional validation needed

**Edge Case 3: XSS Considerations**
- Values inserted directly into template
- Since it's user's own config for their own site, this is expected behavior
- No escaping needed

**Note on Backward Compatibility:**
- Any existing implementation without these properties will use defaults
- Default `showPoweredBy: true` ensures visibility
- Brand changes from "CloserFlow" to "Trietlabs" but this is intentional rebrand

---

## Rollback Plan

If issues arise:

```bash
# Revert both commits
git revert HEAD~1..HEAD

# Or reset to before changes (if not pushed)
git reset --hard HEAD~2
```

Then investigate and re-apply fixes.
