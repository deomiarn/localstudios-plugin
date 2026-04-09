# Blog Overview — `/blog`

## Sections (4, fixed order)

| # | Section | Content |
|---|---------|---------|
| 1 | **Hero** | H1: "Blog — [Company]", kurzer Intro |
| 2 | **Category Filter** | Clean Tabs/Buttons (NOT pill badges), filtert Posts |
| 3 | **Posts Grid** | Cards: Cover Image + Titel + Excerpt + Datum + Kategorie + Autor |
| 4 | **Pagination** | Nummeriert oder "Mehr laden" |

## SEO
- BreadcrumbList: Home > Blog

---

# Blog Category — `/blog/kategorie/[slug]`

## Sections (3, fixed order)

| # | Section | Content |
|---|---------|---------|
| 1 | **Hero** | H1: "[Category] — Blog — [Company]", Kategorie-Beschreibung |
| 2 | **Posts Grid** | Gefiltert nach Kategorie |
| 3 | **Pagination** | Nummeriert |

## SEO
- BreadcrumbList: Home > Blog > [Category]

---

# Blog Post — `/blog/[slug]`

## Sections (6, fixed order)

| # | Section | Content |
|---|---------|---------|
| 1 | **Header** | Titel, Autor + Foto, Datum, Kategorie, Lesezeit |
| 2 | **Cover Image** | Grosses Bild, full-width |
| 3 | **Content** | Portable Text gerendert (Headings, Text, Bilder, Listen) |
| 4 | **Author Box** | Foto, Name, Kurzbio, Link zu /team/[slug] |
| 5 | **Related Posts** | 2-3 verwandte Artikel als Cards |
| 6 | **CTA** | Kontakt oder Newsletter |

## SEO
- BlogPosting Schema + BreadcrumbList: Home > Blog > [Post]
- Author → Person Schema
- publishedAt, dateModified
