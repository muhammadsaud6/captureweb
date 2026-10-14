# Capture Your Curves — static HTML site

A static copy of https://captureyourcurves.wixsite.com/website-2. It needs no Wix account, build step or server code. Upload the whole folder to any web host (cPanel, Netlify, Vercel, GitHub Pages, S3…). `index.html` is the home page.

## Structure

| Path | Contents |
|---|---|
| `*.html` | Desktop pages: 25 menu pages, 3 blog posts, 22 product pages, 3 booking-service pages, the "Book Online" page and a blog category page |
| `mobile/*.html` | Mobile layout of every page (Wix serves a separate phone layout; so does this copy) |
| `assets/img`, `assets/video`, `assets/fonts` | All images, background videos and fonts, served locally |
| `assets/css/site.css` | Overrides for the static copy (hides the Wix banner, fixes for video/backgrounds, lightbox) |
| `assets/css/*.css` | The original Wix stylesheets, with all URLs pointing to local files |
| `assets/js/site.js` | Replaces the Wix runtime: dropdown + mobile menus, background videos, FAQ accordions, gallery fit + lightbox, forms |

Phones (screen narrower than 768px) are sent to `mobile/<page>` automatically, and larger screens are sent back. Add `?desktop` or `?mobile` to a URL to force one layout.

## Desktop and mobile pages

The separate files in `mobile/` are intentional: this Wix export has a desktop and a phone layout for each page. When changing page content, make the same change in both files. The mobile folder also contains some extra files ending in ` - Copy.html`; they are leftover exports and are not used by the site's normal page links.

## Publish on GitHub Pages

This is a static site; it does not need Node.js, a build step, or a server. The included GitHub Actions workflow deploys the site when you push to the `main` branch.

1. Install Git for Windows if `git` is not recognized in the VS Code terminal.
2. Create an empty repository on GitHub, then open this folder in the VS Code terminal and run:

	```powershell
	git init -b main
	git add .
	git commit -m "Prepare static site"
	git remote add origin https://github.com/USERNAME/REPOSITORY.git
	git push -u origin main
	```

	Replace `USERNAME/REPOSITORY` with the GitHub account and repository name. GitHub may ask you to sign in in a browser.
3. In the repository on GitHub, open **Settings > Pages** and set **Source** to **GitHub Actions**. The workflow will publish the site after each push; its public URL appears in the workflow run.

The local Claude settings and the unrelated WhatsApp audio file are excluded from Git. The website's own images, fonts, and videos remain included.

## Things that needed the Wix backend

These look the same as before, but they work differently now:

- **Forms** (newsletter on Home, review form on Testimonials) open the visitor's e-mail app, addressed to `captureyourcurves@gmail.com` with the entries filled in. To collect submissions directly, point the form at a service such as Formspree or Netlify Forms. The address is set in `FORM_EMAIL` in `assets/js/site.js`.
- **Book Now buttons** still link to the Wix booking calendar on `www.captureyourcurves.com/booking-calendar/...`. That works only while bookings stay on Wix. Otherwise, replace those links with another booking tool such as Calendly, Square or Vagaro.
- **Shop**: product pages show the products and prices, but the cart and checkout need a store (for example Shopify Buy Button, Square or Stripe links).
- **Log In / member area, Wix Chat, blog likes and view counters** are display-only.
- **Google Map** (Home) uses a standard Google Maps embed for 170 Jericho Turnpike, Floral Park, NY.
- **Custom embeds** that the site owner added in Wix are kept as small pages in `assets/embed/`. They load the same third-party widgets as before: the Contact Us map, the LeadConnector booking widget (Contact Us), the LeadConnector intake form (Privacy Policy page) and the Cherry payment-plan widget (Payment Plans).
- **Packages** pricing table is a POWR widget loaded from powr.io. It keeps working as long as the POWR app stays active.

## Notes

- The original Wix copy had `noindex`, which tells search engines not to list the site. It has been removed so the live site can appear in Google.
- The footer text reads "©2023 by CYC BC." (the "Proudly created with Wix.com" part was removed). Edit it in each page's footer if it should change.
- The promo popup (Wix lightbox) opens once per browser session on whichever page a visitor lands, and links to Contact Us. To change the promotion, replace `assets/img/popup-promo.jpg` (and its `alt` text in `initPromoPopup` in `site.js`). Add `?nopopup` to a URL to skip it.
- The desktop menu shows every page (no "More" item); links wrap onto extra centred rows and the header grows to fit (`initMenuWrap` in `site.js`, `.cyc-hdr-nav` in `site.css`).
- Every footer ends with a right-aligned "Designed by Muhammad Saud Javed" line (`.cyc-credit`, styled in `assets/css/site.css`).
- The Home page slideshow lists its 8 images in the `data-cyc-slides` attribute of the gallery (in `index.html` and `mobile/index.html`). `site.js` changes the image every 4 seconds.
- Gallery: 62 items, as on the live site. Video items have a `data-cyc-video` attribute pointing to the clip in `assets/video/`; the lightbox plays it.
- Video players: FAQ uses YouTube embeds; Men Section and Red Light Therapy play local MP4 files from `assets/video/`.
- Product pages with more than one image (Omnitrition Drops) switch images with the thumbnails (desktop), dots or swipe (mobile) — `initProductGallery` in `site.js`.
- Page text can be edited directly in the HTML files. Each page has a desktop file and a mobile file, so update both.
