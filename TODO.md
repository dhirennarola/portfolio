# TODO

Everything in the original list — repo rename, domain purchase, DNS, HTTPS,
GA4 — is done. The site is live at https://dhirennarola.com, all 24 pages
return 200, `robots.txt` allows every crawler, and the sitemap is complete.

What remains is **discovery**: search engines cannot rank a site they have not
indexed, and nothing in the codebase can force indexation. These are account
actions only Dhiren can take.

## 1 · Google — Search Console (the blocker for Google results)

Google does not participate in IndexNow, so the sitemap has to be submitted by
hand once. Until this is done, Google has no reason to crawl the site.

- [ ] Add `dhirennarola.com` as a **Domain property** at
      https://search.google.com/search-console (DNS TXT verification — the
      record goes at the registrar/Cloudflare, not in this repo).
- [ ] Submit `https://dhirennarola.com/sitemap-index.xml` under **Sitemaps**.
- [ ] Run **URL Inspection → Request indexing** on `/` first, then `/about`,
      `/services`, `/work`. Do the rest as the first ones get picked up.
- [ ] Check **Pages → Why pages aren't indexed** after ~a week. "Discovered –
      currently not indexed" on a new domain is normal and means keep waiting;
      "Crawled – currently not indexed" means the page needs stronger content
      or links.

## 2 · Bing — Webmaster Tools (feeds DuckDuckGo and Yahoo)

`public/BingSiteAuth.xml` is already served, so verification is one click.
DuckDuckGo and Yahoo both source results from Bing — fixing Bing fixes all
three. Brave runs its own index but also honours IndexNow.

- [ ] Verify `dhirennarola.com` at https://www.bing.com/webmasters (the
      XML-file method — the file is already deployed).
- [ ] Submit `https://dhirennarola.com/sitemap-index.xml`.
- [ ] Confirm the IndexNow key is accepted (key file:
      `/378058de20b41390bb8917d379894ee8.txt`). The deploy workflow pings
      IndexNow automatically on every push to `main` — see
      `.github/workflows/deploy.yml`.

## 3 · Links pointing at the domain

A new domain with no inbound links is slow to index no matter how clean the
markup is. Each of these is a crawlable path to the site:

- [ ] LinkedIn profile → Contact info → Website = `https://dhirennarola.com`.
- [ ] LinkedIn headline + experience updated to match the site
      (22 implementations, 13 countries, Synodica ended Jul 2026,
      independent 2026–).
- [ ] GitHub profile README + profile "Website" field.
- [ ] Odoo App Store publisher profile.
- [ ] Any Upwork/Toptal/community profile already in use.

## 4 · Cloudflare settings to check

- [ ] **Scrape Shield → Email Address Obfuscation.** Cloudflare rewrites the
      `mailto:` on the contact page and footer into
      `/cdn-cgi/l/email-protection`, which leaves no-JS visitors and AI
      crawlers with a dead link. The templates now wrap those links in
      `<!--email_off-->`, which exempts them — verify after the next deploy
      that `hello@dhirennarola.com` appears in view-source. If it still does
      not, turn the feature off entirely.
- [ ] **Bot Fight Mode / AI Scrapers & Crawlers.** If either is on, verified
      AI crawlers get challenged even though `robots.txt` welcomes them.
      Confirm both are off if you want ChatGPT, Claude, Perplexity and Brave
      to read the site.

## 5 · Content facts still marked unconfirmed

`src/content/work/*.md` carries `# TODO(Dhiren): confirm …` comments on facts
that are published live. Resolve or remove each:

- [ ] `elevator-maintenance-automation.md` — Odoo version + implementation year.
- [ ] `it-services-billing.md` — implementation year.
- [ ] `jewellery-sales-tags.md` — country, and whether the client may be named.
- [ ] `ksa-automotive-erp.md` — implementation year/duration.

## 6 · Nice to have

- [ ] The homepage country grid hardcodes its 9 visible countries and the
      "+ Egypt, Australia, Mexico, Laos" overflow, while `countriesServed` in
      `src/data/facts.ts` holds the canonical 13. The continent count is now
      derived from that list, but the grid is not — adding a country means
      editing two places. Worth wiring the grid to the data.
- [ ] Calendly still uses the `dhnarola12` account slug. Works fine; rename
      only if you want the URL cleaner.
