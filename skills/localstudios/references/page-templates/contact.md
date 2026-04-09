# Contact Page Template

## Route: `/kontakt`

## Sections
1. **Hero** — H1: "Kontakt — [Company] [City]", welcoming intro
2. **NAP Block** — Address, Phone (clickable), Email, prominent
3. **Contact Form** — Fields from interview, clear labels, submit CTA
4. **Google Maps** — Embed (URL from interview), full-width, rounded corners
5. **Opening Hours** — Table format, matches schema
6. **Additional Contact** — WhatsApp, Social, "Lieber anrufen? [phone]"

## SEO
- ContactPage signals
- LocalBusiness schema (duplicate from home)
- NAP identical to all other pages
- BreadcrumbList: Home > Kontakt

## Images
- Office/storefront exterior
- Google Maps embed counts as visual element

## Form
```tsx
// Fields from interview, typical:
- Name (required)
- Email (required)
- Phone (optional)
- Subject (select or free text)
- Message (textarea, required)
- Submit: "Nachricht senden"
```
