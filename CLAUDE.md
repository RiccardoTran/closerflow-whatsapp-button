# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a WhatsApp chat widget that can be embedded on websites to provide direct WhatsApp messaging functionality. The widget is distributed via CDN (jsdelivr) and branded as "CloserFlow - WhatsApp Chat Widget for Business" (closerflow.com).

## Architecture

The project consists of three main JavaScript files that work together:

### embed.js (Entry Point)
- Creates a Shadow DOM container to isolate widget styles from the host page
- Loads the widget.js script dynamically from the configured baseUrl
- Injects SEO elements (backlink and schema.org markup) for CloserFlow branding
- Merges user configuration with default values
- Default baseUrl: `https://cdn.jsdelivr.net/gh/YOUR_GITHUB_USERNAME/closerflow-whatsapp-widget@main`

### widget.js (Core Widget Logic)
- Defines all CSS styles in a scoped `:host` context for Shadow DOM
- Creates the floating WhatsApp button and popup chat preview interface
- Implements click handlers for opening/closing the popup
- Generates WhatsApp deep links using `wa.me/{phoneNumber}?text={prefillMessage}` format
- Exports `initWhatsAppWidget(shadowRoot, config)` to global scope

### Configuration Object (window._whatsappConfig)
Users configure the widget by setting properties on `window._whatsappConfig` before loading embed.js:
- `buttonName`: Button text label
- `buttonIconSize`: Icon size in pixels
- `brandImageUrl`: Logo URL for chat header
- `brandName`: Business name displayed in chat
- `brandSubtitleText`: Subtitle text (e.g., "Typically replies within seconds")
- `buttonSize`: 'small', 'medium', or 'large'
- `buttonPosition`: 'left' or 'right'
- `callToAction`: Text on the "Start Chat" button
- `phoneNumber`: WhatsApp number (international format without +)
- `welcomeMessage`: Initial message from business
- `prefillMessage`: Pre-filled message from user
- `baseUrl`: Override CDN location (for local development)

## Development Commands

### Local Testing
```bash
node server.js
```
Starts a local HTTP server on port 3001. Visit http://localhost:3001/ to see test.html with the widget loaded locally.

### Testing Configuration
- test.html uses `baseUrl: '.'` to load widget.js and embed.js from the local directory
- Production embeds should use the jsdelivr CDN URL or omit baseUrl entirely

## Key Implementation Details

### Shadow DOM Isolation
The widget uses Shadow DOM (`attachShadow({ mode: 'open' })`) to prevent CSS conflicts with the host page. All styles are scoped within the shadow root.

### Event Handling
- Widget button toggles popup visibility
- Close button (×) in popup header closes the popup
- Document-level click listener closes popup when clicking outside (uses `event.composedPath()` to detect clicks within Shadow DOM)

### SEO Elements
The embed.js file automatically injects:
1. Visually hidden backlink with proper ARIA attributes
2. Schema.org JSON-LD structured data for the SoftwareApplication

### WhatsApp Link Format
Links use the format: `https://wa.me/{phoneNumber}?text={encodeURIComponent(prefillMessage)}`

## File Structure Notes
- No build process required - pure vanilla JavaScript
- No package.json dependencies for runtime (Node.js server.js uses built-in modules only)
- Files are designed to be served directly from CDN
