# Trietlabs WhatsApp Chat Widget

A customizable WhatsApp chat widget that can be embedded on any website. Built by Trietlabs - https://www.trietlabs.com/

## Installation

Add this code to your website before the closing `</body>` tag:

```html
<script>
    window._whatsappConfig = {
        buttonName: 'Message Us',
        buttonIconSize: 22,
        brandImageUrl: 'YOUR_LOGO_URL',
        brandName: 'Your Business Name',
        brandSubtitleText: 'Typically replies within seconds',
        buttonSize: 'large',
        buttonPosition: 'right',
        callToAction: 'Start Chat',
        phoneNumber: 'YOUR_PHONE_NUMBER',
        welcomeMessage: 'Hi there 👋',
        prefillMessage: 'Hi, I want to know more!'
    };
</script>
<script src="https://cdn.jsdelivr.net/gh/RiccardoTran/closerflow-whatsapp-widget@main/embed.js" async></script>
```

## Configuration Options

- `buttonName`: Text displayed on the floating button
- `buttonIconSize`: Size of the WhatsApp icon in pixels
- `brandImageUrl`: URL to your business logo
- `brandName`: Your business name
- `brandSubtitleText`: Subtitle text (e.g., "Typically replies within seconds")
- `buttonSize`: 'small', 'medium', or 'large'
- `buttonPosition`: 'left' or 'right'
- `callToAction`: Text on the "Start Chat" button
- `phoneNumber`: WhatsApp number in international format without + (e.g., 12184273128)
- `welcomeMessage`: Initial greeting message
- `prefillMessage`: Pre-filled message from user
- `showPoweredBy`: Show/hide the "Powered by" footer (default: true)
- `poweredByBrand`: Brand name displayed in the footer (default: 'Trietlabs')
- `poweredByUrl`: URL for the brand link in footer (default: 'https://www.trietlabs.com')

## Customizing the "Powered By" Footer

By default, the widget displays "⚡ Powered by Trietlabs" in the footer. You can customize or hide this:

**Hide the footer completely:**
```javascript
window._whatsappConfig = {
    // ... other config ...
    showPoweredBy: false
};
```

**Customize the brand:**
```javascript
window._whatsappConfig = {
    // ... other config ...
    showPoweredBy: true,
    poweredByBrand: 'Your Brand',
    poweredByUrl: 'https://yourbrand.com'
};
```

## Local Development

```bash
node server.js
```

Visit http://localhost:3001/ to test the widget locally.
