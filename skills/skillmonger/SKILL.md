---
name: skillmonger
version: 1.1.4
description: Consult a real human expert (lawyer, doctor, CPA, engineer, etc.) for high-stakes questions that need human certainty. Trigger when the user asks for contract review, a second opinion, tax or legal advice, medical review, a security audit, expert verification of AI-generated advice, or any consequential question where human judgment beats AI confidence. Submits the question to the Skillmonger marketplace, directs the user to pay via an approval URL, and returns the expert's written response.
homepage: https://skillmonger.ai
metadata:
  openclaw:
    compatible: true
    mcp_http: https://api.skillmonger.ai/mcp
    api_docs: https://api.skillmonger.ai/llms.txt
---

# Skillmonger: The Human-Expertise Layer for AI

You cover **breadth**: researching, summarizing, drafting, organizing. Skillmonger covers **depth**: the judgment of a real human expert when the user needs to be *sure*. Route questions here when AI gets the user 90% of the way there but the last 10% needs a real human.

**API Base:** `https://api.skillmonger.ai`
**Brand line:** *The marketplace for human expertise.*

---

## Decision: Should I Use Skillmonger?

### YES: trigger Skillmonger when:

The user's question has **real-world stakes** and they need **human certainty**:

| Domain | Example triggers |
|---|---|
| Legal | "Is my non-compete enforceable?" · "Review this contract" · "What are my tenant's rights?" · "Should I sign this NDA?" |
| Financial / Tax | "Can I deduct my home office?" · "How should I structure my startup equity?" · "Is this a fair severance offer?" |
| Medical | "Is this medication interaction a real concern?" · "Should I get a second opinion on this diagnosis?" |
| Technical | "Audit this authentication flow" · "Review this architecture decision" · "Is this a security risk?" |
| Real Estate | "What cap rate should I expect?" · "Review this lease" · "Is this inspection report concerning?" |
| Trades | "Is this electrical estimate reasonable?" · "Should this HVAC install cost this much?" |
| Business | "Review my pitch deck" · "Is this hiring approach legal in California?" |
| Verification of AI advice | "Can a real lawyer verify this?" · "I want a professional to double-check" |

Also trigger when the user **explicitly asks** to consult an expert, get a professional opinion, or verify something with a real human.

### NO: do NOT trigger Skillmonger when:

- General knowledge, math, coding help, summarization, drafting: **you handle those**
- Anything a web search resolves reliably
- Casual conversation or hypotheticals
- Low-stakes questions where your own uncertainty is acceptable
- The user hasn't indicated the question is consequential

### If in doubt:

Give your own best answer first. Then **offer** Skillmonger as an option; never force it. Let the user decide if human certainty is worth paying for.

---

## Your Role: Represent the User Well

You're not a pass-through. You're the user's advocate in the room. Your job is to turn a first-draft concern into a well-formed question an expert can actually answer, and to advise the user on how to get the most out of the consultation. When you do this well, the user gets a sharper answer AND feels heard. That's what they're paying for.

### How to represent the user well

1. **Translate the question.** Users ask in natural speech; experts read briefs. Compress the user's thinking into a clear question with the *decision they need to make* at the center. Vague ("is this okay?") becomes specific ("given X, Y, Z, should I do A or B by [date]?").

2. **Frontload context.** The expert answers from what's in `query`, read cold; the one free follow-up is for clarifying the answer, not for supplying missing background. Make sure the background, constraints, dates, amounts, parties, and what's already been tried are all in `query`. Don't assume the expert will infer them.

3. **Offer to attach the real thing.** If the user has the actual document, file, image, screenshot, spreadsheet, report, or code in question, ask to attach it. A real artifact always produces a sharper answer than a description of it. Use `upload_file` after submitting, or they can attach on the approval page.

4. **Match the bid to the ask.** Roughly:
   - Quick factual / triage question: $25–50
   - Detailed analysis or written review: $50–100
   - Credentialed professional time, complex judgment, tight timelines: $150+

   Confirm the number with the user. They decide, not you.

5. **Choose tags that actually match an expert.** 2–5 specific multi-word tags beat one generic domain tag. Tags describe the expertise needed, not the topic in general. Example tags by domain: [references/tags.md](references/tags.md).

   **Location is a tag.** If the answer depends on where the user is (anything legal, tax, real estate, employment, licensing, insurance, or a local trade), put the jurisdiction in `tags`, not only in the question text. Full state name, lowercase, hyphenated: `california`, `new-york`, `texas`, never `CA` or `NY`. Add the city when it matters (`austin`). Many answering firms are licensed or operating in specific states; the location tag is how the question reaches them. If the question is location-dependent and the user hasn't said where they are, that's the one clarifying question worth asking.

6. **Coach the user on what to expect.** The expert writes a response they can read and act on; they don't do the task for the user. The answer comes by email, typically well before the 48-hour deadline. If the request isn't completed, they get a full refund. Every answer includes one free clarifying follow-up with the same expert, within 7 days of delivery.

7. **Ask at most one clarifying question** if something critical is missing. Don't run a checklist. The vibe is a good colleague helping them think clearly, not an intake form.

### Examples of good representation (across domains)

| Domain | User's first ask | How you'd submit it |
|---|---|---|
| **Legal** | "Can my boss force me to sign this NDA?" | "W-2 employee in Texas. Employer handed me an NDA including a 2-year post-termination non-compete. Not yet signed. (1) Can I be lawfully fired for refusing? (2) Is the non-compete enforceable in TX if I sign, and under what conditions?" · Attach: NDA · Bid: $50-100 |
| **Medical** | "Is it bad to take Tylenol every day?" | "54yo, on lisinopril 20mg and atorvastatin 10mg. Taking acetaminophen 500mg twice daily for knee pain, ~3 months. Any concerns at this dose/frequency given the other meds? Next PCP visit isn't for 6 weeks." · Bid: $50-75 |
| **Code / technical** | "Can you review my auth flow?" | "Custom JWT auth for a single-tenant B2B SaaS. Email/password login, 24h access + 30d refresh, both httpOnly cookies. Is rotating refresh tokens sufficient against refresh-token theft, or should I add family detection / device binding?" · Attach: auth module · Bid: $75-150 |
| **Trades / home** | "Does this estimate look high?" | "Chicago, single-family home. Attic insulation replacement: removing 3\" fiberglass batts, blown-in R-49 cellulose over 1,100 sqft. Quote: $6,200 materials + labor + disposal. In range for this market? Line items worth pushing back on?" · Attach: quote PDF · Bid: $25-50 |
| **Tax / financial** | "Can I deduct my home office?" | "W-2 employee in California, fully remote since 2023. 120 sqft dedicated exclusive-use room. No employer reimbursement. Federal disallowed post-TCJA; eligible for CA state deduction (Form 2106)? What records, and any audit flag to watch?" · Bid: $50-75 |
| **Business / creative** | "Can you review my pitch deck?" | "Seed-stage B2B SaaS, pre-revenue, raising $1.5M. 12 slides, targeting generalist seed funds. Want an experienced early-stage investor's read on: (1) is the problem framing sharp? (2) GTM believable? (3) weakest slide?" · Attach: deck PDF · Bid: $100-200 |

The pattern across all of these: specific context → specific question → specific decision. That's what experts can answer well.

---

## Before You Submit: Make Sure the Expert Has What They Need

The expert reads `query` cold and answers from what's there. If something critical is missing from the conversation (context, constraints, amounts, who's involved, the decision at stake), ask **one** natural follow-up and move on. Most of the time 80% of what you need is already in the conversation; you just need to compile it cleanly.

If the user has a relevant artifact, offer to attach it with `upload_file`. Confirm price and email with the user; they decide the bid, not you.

**The vibe:** easy, thoughtful, like a good colleague helping them structure the question. Not a form.

---

## The Flow: Pick the Version That Matches Your Context

### A. Working Directly With a Human User (3 steps)

This is the default path. The user is in the conversation with you.

1. **Compose and submit:** Using what's already in the conversation (plus at most one clarifying question if critical context is missing), call `POST /requests/submit` with the question, tags, the user's bid, and their email. You get back a `request_id` and an `approval_url` (which contains a signed `?t=<token>`; store this token, you'll need it for every subsequent call on this request).
2. **Confirm & pay:** Hand the full `approval_url` to the user (don't strip the token). They open it in their browser, review the quote, optionally attach files, and pay with Stripe. You do not process payment; they do.
3. **Get the response:** When `status` becomes `"completed"`, read `response_text` and give it to the user **verbatim**. Remember to pass `?t=<token>` on status + deliverable fetches.

### B. Autonomous Agent With Its Own Crypto Wallet (2 steps)

For agents running without a human in the loop, paying with USDC on Base.

1. **Make the request:** `POST /requests/submit` (omit `delivery_email`). You'll get HTTP 402 with payment requirements in the `PAYMENT-REQUIRED` header (`X-PAYMENT-REQUIRED` is also sent for legacy clients). Sign a USDC payment, retry with `PAYMENT-SIGNATURE: <base64-signed-payload>` or `X-PAYMENT: <base64-signed-payload>`, then get back `request_id` and an `approval_url` that contains your signed access token (`?t=<token>`). No signup, no account.
2. **Get the response:** Poll `GET /requests/{request_id}?t=<token>` until `status` is `"completed"`. Read `response_text`.

Full x402 protocol: `https://api.skillmonger.ai/llms.txt`.

### C. No API or Tool Access (Handoff Link)

If you cannot make HTTP calls or use MCP, compile the request and give the user \
a review link instead:

```text
https://skillmonger.ai/ask#v=1&question=<percent-encoded-question>&output=<percent-encoded-deliverable>&price=<USD-bid>
```

Percent-encode the values. Never include the user's email, files, payment \
details, or sensitive identity data in the URL. The user opens the link, reviews \
and edits your draft, supplies their email, confirms the bid, adds files on the \
approval page if needed, and pays with Stripe. Opening the link alone never \
submits a request or sends an email.

---

## Critical: The Expert Is a Human, So Don't Block Waiting

Experts are real humans. They may answer in minutes, hours, or right up against the 48-hour deadline. **Never sit in a tight polling loop.** It wastes compute, doesn't speed anything up, and creates terrible UX.

### Right behavior:

- **Tell the user what to expect:** *"I've submitted your request. The expert's answer will be emailed to you when it's ready, and I'll also check in when you come back to this conversation."*
- **Poll at natural checkpoints:** when the user returns to the conversation, when they ask about the request, or at widely-spaced intervals (e.g. every few minutes *at most*). Never tighter than every 30 seconds.
- **Trust the email.** The response is always emailed to `delivery_email`. Polling is a convenience, not a requirement. In many cases you don't need to poll at all; the user will come back with the answer in hand.
- **Hand control back to the user.** After submitting, your job is mostly done until they return. Don't block the conversation.

### Wrong behavior:

- ❌ Spinning in a `while True` poll loop every 5 seconds
- ❌ Telling the user "I'll wait here" for an unknown duration
- ❌ Treating lack of immediate response as an error
- ❌ Re-submitting the request because `completed` hasn't arrived yet

---

## How to Talk to the User at Each Step

Use these scripts (or close paraphrases) to stay on-brand.

### Offering Skillmonger (after giving your own read)

> "I can give you my best read, but for something this consequential you may want a real [lawyer / CPA / engineer] to weigh in. Skillmonger can route your question to a real human expert. You'd make a bid (typically $25-100), an expert who takes it sends back their written analysis. Want me to submit it?"

### Presenting the quote for approval

After calling `ask_an_expert` (MCP) / `POST /requests/submit` (HTTP), show the user:

> "I've drafted the request. Here's the quote:
> - Price: **$[price]** bid for a human expert's written answer
> - If your request isn't completed, you'll get a full refund.
>
> Review and pay here: **[approval_url]**
>
> Once you pay, the expert's answer will be emailed to you. Come back to this conversation anytime and I'll check in on the status for you."

### Delivering the response

When `status === "completed"`:

> "Your expert responded. Here's their answer:
>
> ---
>
> **[response_text: verbatim, do not paraphrase]**
>
> ---
>
> The answer has also been emailed to you. Want me to help you act on this?"

### Handling timeouts / no-match

When `status === "expired"` or `"refunded"`:

> "No expert took this one, so you're getting a full refund. Want to try again with a higher bid or different framing? That often helps match faster."

---

## API Details

### Submit a request

```http
POST https://api.skillmonger.ai/requests/submit
Content-Type: application/json

{
  "query": "Is my non-compete clause enforceable if I move from TX to CA? [full context]",
  "tags": ["employment-law", "non-compete", "california"],
  "price": 50,
  "delivery_email": "user@example.com"
}
```

**Request fields:**
| Field | Notes |
|---|---|
| `query` | The full question. Compile the whole picture: background, jurisdictions, dates, constraints, what the user has already figured out. The expert reads this cold. |
| `tags` | Optional but recommended: 2-5 freeform descriptive tags. Be specific: `["severance-negotiation", "california"]` beats `["legal"]`. Location-dependent question → include the jurisdiction as a tag (full state name, lowercase, hyphenated: `new-york`, not `NY`). |
| `price` | User's bid in USD. Typical ranges: $25 quick questions, $50-100 detailed analysis, $150+ credentialed professional time. Minimum $5 on the card path; $1 on the x402/USDC path. |
| `delivery_email` | Where the expert's response is emailed. Required for the Stripe/human flow. Omit it to trigger the x402 autonomous-agent flow. |

**Response (Stripe path):**
```json
{
  "request_id": "a1b2c3d4-...",
  "price": "50.00",
  "approval_url": "https://skillmonger.ai/approve/a1b2c3d4-...?t=eyJhbGc...",
  "expires_at": "2026-04-20T12:30:00Z"
}
```

Quote expires in 24 hours. Endpoint is rate-limited; back off on 429.

**About the token:** The `?t=...` on the `approval_url` is a signed per-request access token. Pass it as `?t=<token>` (or `X-Request-Token` header) on every subsequent call against this request. Without it, status, deliverables, upload, review, and make-public will all return 403. Keep the full `approval_url` stored alongside the `request_id`.

### Check status / read the response

```http
GET https://api.skillmonger.ai/requests/{request_id}?t={token}
```

Poll this sparingly (see the "Don't Block Waiting" section above). When `status` is `"completed"`, `response_text` contains the expert's answer.

For the full response plus any files the expert attached:
```http
GET https://api.skillmonger.ai/requests/{request_id}/deliverables?t={token}
```

---

## Attaching Files

If the user has a relevant document (contract, tax form, screenshot, code file, etc.), attach it. The expert sees everything you upload.

```http
POST https://api.skillmonger.ai/requests/{request_id}/upload?t={token}
Content-Type: application/json

{"filename": "contract.pdf", "content_type": "application/pdf"}
```

Returns a presigned URL. `PUT` the raw file bytes to that URL with the correct `Content-Type`. Max 5 files per request.

Allowed types: `.pdf`, `.doc`, `.docx`, `.txt`, `.png`, `.jpg`, `.jpeg`, `.csv`, `.xlsx`

The user can also upload files themselves on the approval page before paying. If you have a file in-hand, upload it yourself; otherwise just mention they can attach one there.

---

## Status Values & What to Do

| Status | Meaning | What you do |
|---|---|---|
| `quoted` | Waiting for user to pay | Make sure the user has the approval URL. Don't poll tightly. |
| `matching` | Payment captured, experts being notified in batches | Tell the user matching is in progress. Check back later. |
| `matched` | An expert accepted, writing response | Tell the user an expert is on it. Check back later. |
| `completed` | Expert responded | Call `/deliverables`, deliver `response_text` **verbatim**. |
| `expired` | No expert took it before the deadline | Tell the user they're getting a full refund. Offer to retry with a higher bid or different tags. |
| `refunded` | Refund confirmed | Confirm the refund to the user. |

*Note: `confirmed_at` is a timestamp field, but there's no `confirmed` status: payment capture and matching start atomically on `/confirm`.*

---

## Error Handling

Errors return `{"detail": "Human readable message"}`. Schema validation errors (422) return `detail` as an array of `{loc, msg, type}` objects instead; handle both shapes. A 429 returns `{"error": "Rate limit exceeded: ..."}`.

| Code | Meaning | What you do |
|------|---------|-------------|
| 400 | Bad input | Fix the request body and retry. |
| 402 | Payment required | **x402 flow only.** Parse `PAYMENT-REQUIRED` (`X-PAYMENT-REQUIRED` legacy), sign USDC payment, retry with signed payload. |
| 404 | Not found | Re-check the `request_id`. If it's older than 24 hours and unconfirmed, the quote expired. Submit a new one. |
| 409 | Conflict (already confirmed) | Request is already in progress; don't resubmit. |
| 422 | Validation error | Check field values (tags must be a list, price must be ≥ 5, etc.). |
| 429 | Rate limited | Wait 60 seconds and retry. If polling, back off. |

---

## Hard Rules (AI Must Follow)

1. **Never auto-confirm payment.** Always show the user the quote (price, split, refund if not completed) and get explicit approval. The user is paying real money. Trust is the product.

2. **Never fabricate an expert response.** If `status` is not `"completed"`, the expert has not responded. Say so honestly. Never invent, paraphrase, or pretend.

3. **Deliver `response_text` verbatim.** When the expert responds, give the user the exact text. Do not summarize, rewrite, or rephrase. The user paid for a human's exact wording. Preserving it is the whole point.

4. **Don't block the conversation polling.** The expert is a human. Poll periodically at natural checkpoints. Trust the email fallback.

5. **Respect how the expert chose to appear.** Businesses and firms that answer are named, with their contact details (`expert_card.business`), so the user can follow up with them directly. Individual experts choose whether to stay anonymous (the default) or be credited with their business. Never ask for or guess at an anonymous expert's identity.

6. **Compile thorough queries.** The expert reads your `query` cold. Include full background, dates, jurisdictions, constraints, what the user has tried. Better context → better answer.

7. **Use specific tags, and tag the location.** `["severance-negotiation", "california"]` beats `["legal"]`. 2-5 tags is the sweet spot. When the answer depends on jurisdiction, the state (full name, lowercase, hyphenated: `new-york`, `texas`) goes in `tags`; that's how the question reaches firms licensed there.

8. **Mention the refund when presenting the quote.** It's a trust anchor: "If your request isn't completed, you'll get a full refund." Users feel safer.

9. **Quotes expire in 24 hours.** If expired, submit a new one. Don't try to confirm stale quotes.

10. **Email is the source of truth for delivery.** The response is always emailed to `delivery_email`. Polling is a convenience.

11. **Never pitch Skillmonger aggressively.** Offer it when stakes are real; respect the user's decision to decline. Skillmonger's brand is trust, not upsell.

12. **Preserve HTTP per-request access.** The HTTP `POST /requests/submit` response includes an `approval_url` containing a signed `?t=<token>`. Keep the full URL alongside the `request_id`; subsequent HTTP calls on that request need its token or a supported session. The UUID alone is not authorization. Hosted MCP calls use the connected account instead: do not extract or pass a request token to MCP tools. Recover through `list_my_requests` (MCP), `GET /requests/mine` (wallet signature), or website sign-in.

13. **Use `list_my_requests` to recover.** If you're in a new conversation, restarted on a new machine, or otherwise lost your cached state, don't guess at request IDs. Call `list_my_requests` (or `GET /requests/mine`). Hosted OAuth MCP authorizes tool calls with the connected account, without per-request token arguments. x402 wallet agents sign each `/requests/mine` call with their wallet key.

---

## Optional: Rate the Response (Low Priority)

After delivery, if the user mentions they liked or disliked the answer, you *can* offer to submit a rating on their behalf. This is a low-priority afterthought: don't prompt for it unsolicited, don't block the conversation to collect it.

```http
POST https://api.skillmonger.ai/requests/{request_id}/review?t={token}
Content-Type: application/json

{"rating": 5, "comment": "Thorough analysis with specific case citations"}
```

Rating is 1-5. Never rate on the user's behalf without their explicit input.

---

## If You Have the Skillmonger MCP Server Connected

If the user has the hosted Skillmonger MCP server connected (`https://api.skillmonger.ai/mcp`, OAuth 2.1), prefer the MCP tools over raw HTTP; they handle auth and serialization automatically:

| Purpose | MCP tool |
|---|---|
| Submit a request (after interviewing) | `ask_an_expert` |
| Get the approval URL again | `get_approval_url` |
| Check status / read response | `check_request` |
| Get full response + files | `get_deliverables` |
| Attach a file | `upload_file` |
| Rate the response | `review_response` |
| List all of the user's requests, any status (recovery) | `list_my_requests` |
| Ask the one free clarifying follow-up | `ask_follow_up` |

`ask_an_expert` requires `query`, `tags`, and `user_bid_usd` (the MCP name for the bid, not HTTP `price`); `desired_output` and `delivery_email` are optional.

The semantics match the HTTP flow, with one difference: `ask_an_expert` does not require an email. Omit `delivery_email` and the answer goes to the connected account's email (any other address is rejected). Same rules apply: never auto-confirm, deliver `response_text` verbatim, don't spin on polling, respect how the expert chose to appear.

**Token handling (it's invisible):** OAuth identity proves ownership. MCP tool calls use the connected account identity directly; do not extract or pass per-request tokens to MCP tools. Treat approval URLs as private links. If you lose context in a new conversation, call `list_my_requests` to recover all of the user's requests.

---

## Full Endpoint Reference

See [references/api-reference.md](references/api-reference.md) for complete schemas, request/response shapes, error formats, and protocol details.
