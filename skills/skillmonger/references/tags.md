# Skillmonger Tags & Expertise Areas

Skillmonger uses **freeform tags**; there are no fixed categories. You define the expertise needed with descriptive tags, and the platform matches you with the best-fit expert using semantic search.

## How Tags Work

- Include 2-5 descriptive tags with each request
- Tags are lowercase, free-form strings
- More specific tags = better expert matching
- The AI matching system uses embeddings to find experts regardless of exact tag wording

## Location Tags

Many answering experts are firms licensed or operating in specific states.
When the answer depends on where the user is (legal, tax, real estate,
employment, licensing, insurance, local trades), **add the jurisdiction as
its own tag** so the question reaches them:

- Full state name, lowercase, hyphenated: `california`, `new-york`, `texas`,
  `north-carolina`
- Never abbreviations: `CA`, `NY`, `TX` won't match anything
- Add the city when it matters: `austin`, `brooklyn`, `san-francisco`
- Multi-state situations get multiple tags: `["employment-law", "remote-work", "california", "new-york"]`
- If the question is location-dependent and the user hasn't said where they
  are, ask. It's the one clarifying question that reliably pays off

Example: `["startup-formation", "delaware-c-corp", "california"]` for a
California founder asking about incorporating in Delaware.

## Example Tags by Domain

| Domain | Good Tags |
|--------|-----------|
| Legal | `legal`, `employment-law`, `contracts`, `non-compete`, `startup`, `ip` |
| Finance | `tax`, `accounting`, `financial-planning`, `investment`, `bookkeeping` |
| Real Estate | `real-estate`, `commercial`, `valuation`, `lending`, `appraisal` |
| Medical | `medical`, `dermatology`, `mental-health`, `nutrition` |
| Technical | `architecture`, `security`, `scalability`, `database`, `devops` |
| Business | `gtm`, `sales`, `marketing`, `strategy`, `startup`, `growth` |
| Design | `design`, `ux`, `branding`, `product-design` |
| HR | `hr`, `hiring`, `compensation`, `benefits`, `compliance` |

## Pricing

Skillmonger uses **user-set bidding**: you name your price, experts decide if it's worth their time.

- Typical range: $20–100 per request
- Every request funds access to human expertise; the expert sees their earnings before accepting
- **The only price that matters is the one in the quote response.** Always show it to the user before confirming
- If the request isn't completed, the user gets a full refund
