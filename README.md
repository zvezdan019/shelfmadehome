# Shelf Made Home

Astro blog for shelfmadehome.com, deployed on Cloudflare Pages.

## Run locally

```bash
npm install
npm run dev        # http://localhost:4321
npm run build      # outputs to dist/
```

Requires Node 22+.

## Before the first deploy

Edit `src/consts.ts`:

- `CONTACT_EMAIL`: the address shown on the Contact and Privacy pages.
- `AMAZON_TAG`: your Associates tracking ID, once you're approved.
- `PINTEREST_DOMAIN_VERIFY`: the `content` value of Pinterest's `p:domain_verify` meta tag.
- `CF_ANALYTICS_TOKEN`: optional, from Cloudflare Web Analytics.

Replace the placeholder images:

- `public/images/posts/small-kitchen-organization-ideas.svg`: add your real hero photo (e.g. `small-kitchen-organization-ideas.jpg`, 1200×1500 works well) and update `heroImage` in the post frontmatter.
- `public/og-default.png`: default social share image (1200×630). Your Pinterest cover works.

## Deploy to Cloudflare Pages

1. Push this folder to a new GitHub repo.
2. Cloudflare dashboard → Workers & Pages → Create → Pages → Connect to Git → pick the repo.
3. Build settings: framework preset **Astro**, build command `npm run build`, output directory `dist`.
   Add an environment variable `NODE_VERSION` = `22`.
4. After the first deploy: Custom domains → add `shelfmadehome.com` (and `www.shelfmadehome.com`, redirected to the apex).
   The domain is already on Cloudflare, so DNS is set up automatically.

Every push to `main` deploys automatically. Pull requests get preview URLs.

## Writing a post

Create `src/content/blog/your-post-slug.mdx`:

```mdx
---
title: 'Pantry Organization Ideas for Small Spaces'
description: 'One or two sentences. Used on cards, in search results and in social previews.'
pubDate: 2026-10-01
heroImage: '/images/posts/pantry-organization.jpg'
heroAlt: 'Describe what is in the photo.'
pinImage: '/images/posts/pantry-organization-pin.jpg'   # optional 2:3 pin graphic
draft: false
---
import ProductCard from '../../components/ProductCard.astro';

Your text in Markdown.

<ProductCard name="Clear stackable pantry bins" asin="B0XXXXXXXX">
  One or two sentences on why it helps.
</ProductCard>
```

The file name becomes the URL: `/blog/your-post-slug/`.

### ProductCard

- `asin="…"` builds an Amazon link with your `AMAZON_TAG`, or pass a full `href="https://amzn.to/…"` from SiteStripe.
- Optional `image="/images/products/…"` and `imageAlt="…"`. Use only your own photos or images from Amazon SiteStripe / PA-API.
- No prices: Amazon doesn't allow static prices on your site.
- Cards without a link show no button in production (a reminder appears in `npm run dev`).

Links are rendered with `rel="sponsored nofollow"`, and every post shows the affiliate note at the top.

## Pinterest website claim

Pinterest → Settings → Claimed accounts → Websites → Claim → choose "Add HTML tag".
Copy only the `content` value into `PINTEREST_DOMAIN_VERIFY`, push, wait for the deploy, then click Verify.

## Structure

```
src/
  consts.ts            site settings
  content.config.ts    post frontmatter schema
  content/blog/        posts (.md / .mdx)
  components/          Header, Footer, Mark (logo), PostCard, ProductCard, AffiliateNote, PinButton
  layouts/             BaseLayout (meta, fonts), PageLayout (simple pages)
  pages/               home, ideas, blog/[slug], about, contact, privacy-policy, affiliate-disclosure, 404, rss
  styles/global.css    brand tokens and base styles
public/                favicon, robots.txt, images
```

The privacy policy and affiliate disclosure are a starting point, not legal advice. Review them and keep them accurate if you add analytics, a newsletter or ads.
