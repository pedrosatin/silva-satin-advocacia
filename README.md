# Silva Satin Advocacia

The institutional website of Silva Satin Advocacia, a law firm based in Maringá, Paraná, Brazil, led by Dra. Maria Satin (OAB/PR 97.566). The site presents the firm's practice areas and contact channels for prospective clients.

Live site: [silvasatin.adv.br](https://silvasatin.adv.br/)

## Practice areas

- Direito Imobiliário (Real Estate Law)
- Direito de Família (Family Law)
- Direito das Sucessões (Estate and Succession Law)

## Stack

This is a static site: plain HTML, CSS, and vanilla JavaScript, with no framework, bundler, or build step.

- `index.html`: the landing page, with header/navigation, hero section, about section, practice areas, office location with a lazily loaded Google Maps embed, and footer contact block.
- `privacidade.html`: the privacy policy and LGPD (Lei nº 13.709/2018) notice, covering data handling and third-party cookies such as the Google Maps embed.
- `styles.css`: styling for both pages.
- `assets/`: images and fonts (logo, lawyer portrait, custom fonts).
- `robots.txt`, `sitemap.xml`, `site.webmanifest`, favicons, and social preview metadata (Open Graph, Twitter Card, JSON-LD `LegalService` schema) for SEO and discoverability.

## Running locally

No dependencies or build step are required. Serve the directory with any static file server, for example:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000` in a browser.

## Deployment

The site is deployed as static files to the production domain (`silvasatin.adv.br`). There is no CI/CD pipeline in this repository; changes are published by deploying the contents of the repository root to the hosting provider.

## Compliance

The site includes a dedicated privacy policy (`privacidade.html`) describing how personal data is handled in accordance with Brazil's LGPD (Lei Geral de Proteção de Dados, Lei nº 13.709/2018), including the use of third-party services such as the embedded Google Maps widget.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
