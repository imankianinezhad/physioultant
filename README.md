# Physioultant

**A free, multilingual pain guide that starts from the body map and ends with something useful to do next.**

Live prototype: https://physioultant.configym.com/
Languages: English · فارسی (Persian, right-to-left) · Français
Developer: [Iman Kiani](https://imankiani.com/)
Status: **working prototype, version 1.7.0, in active development**

> Physioultant is an educational tool. It does not diagnose anything, it is not a medical device, and it does not replace a doctor or a physiotherapist.

---

## Why I'm building this

When something hurts, most people don't start with a diagnosis. They start with "my shoulder hurts when I lift my arm" and then spend an evening on search results that are too general, too clinical, or only in English. A lot of good rehab material exists, but it's hard to reach from where a person actually is: a sore body part and a vague description.

Physioultant tries to close that gap. You show it where it hurts, either by tapping an anatomy picture or by simply talking, you say what kind of pain it is, and you get a short list of sensible next steps: reliable reading, exercise videos, what's available over the counter in your country, and a clear summary you can hand to a clinician.

I'm especially interested in people who are poorly served by English-only health content, which is why Persian and French are first-class languages here and not an afterthought.

## What it does today

**1. Find the problem area, two ways**

- *Tap the body.* Four real anatomy images (male and female, front and back) with clickable zones. You go from a broad region (head and neck, arm, trunk, leg) to an area (19 of them, for example shoulder, knee, lower back) to an exact part (64 parts, for example rotator cuff, Achilles tendon, sacroiliac joint).
- *Just say it.* You can describe the problem out loud for up to three minutes (or type it). The text is summarised by an AI model, and the app lists the problem areas it understood. Nothing moves forward until you confirm the list yourself.

**2. Say what kind of pain it is**

Pain options change with the body part: joint pain, muscle pain, bone pain and nerve pain each get their own wording (26 pain types in total). There are also a handful of options that only appear when relevant to the sex you pick in Settings, for example pregnancy-related pelvic pain or a sudden hot, swollen big toe joint. Picking a sex is optional, and "no preference" works fine.

**3. Get a useful result page**

- Searches for exercise, stretching and self-care videos on YouTube, in your language and in English
- Searches on well-known medical sites (NHS, Mayo Clinic, Cleveland Clinic, MedlinePlus, Physiopedia)
- Over-the-counter medicine information based on the country you choose in Settings, with the right pharmacy and health sites for Canada (Rexall, Shoppers Drug Mart, Jean Coutu, Health Canada), the US, the UK and France, plus a map search for nearby pharmacies
- A **prompt builder** that turns your answers into a well-structured question you can paste into any AI chat assistant
- A **doctor summary**: a one-page sheet with your main complaint and a list of the things a clinician will almost certainly ask (when it started, what makes it worse, numbness, fever, medicines, and so on), with blanks to fill in, plus a few questions worth asking at the appointment
- A **text report** with everything above in one file

**4. Basic safety screening**

If the description mentions chest pain, shortness of breath, loss of bladder or bowel control, sudden weakness or numbness, or fever, the app shows a clear warning before going any further. This check is rule-based and runs separately from the AI model, so it doesn't depend on the model getting it right.

## Things I care about

- **Privacy.** There are no accounts and no database. Body-map selections and the reports stay in the browser. The only time data leaves the device (apart from you clicking a link to YouTube or a search engine) is when you choose the voice/text analysis: the text you confirm goes to a small server function and then to Google's Gemini API. The API key lives only in the hosting platform's secret store, never in this repository or in the browser. Details are in `source/SECURITY.md`.
- **Accessibility.** I'm building toward WCAG 2.2 AA: keyboard navigation, screen-reader labels, visible focus, large touch targets, a high-contrast mode, adjustable text size, reduced-motion support, and correct right-to-left layout for Persian. I want to be clear that this is a goal, not a certificate. It hasn't been audited yet (see below).
- **Speed and size.** Each page is a single static file of about 30 KB compressed, with no third-party scripts, fonts or trackers. Only the anatomy image you are looking at is loaded.
- **Honesty about what the app is.** Wherever the app talks about causes or conditions, it says "possible" and points to a professional. The AI is used to organise what the user said, not to diagnose.

## Where things really stand

I'd rather be upfront than have anyone discover this later.

| Area | Status |
|---|---|
| Body map, region → area → part → pain type flow | Built and working |
| English, Persian, French interface | Built; **translations have not been reviewed by native-speaking clinicians** |
| Settings (theme, accent colour, text size, layout, density, contrast, motion, sex, country) | Built |
| Results page, prompt builder, doctor summary, text report | Built |
| Voice/text analysis with AI summary and user confirmation | Built and tested against a simulated AI service; **still needs live testing with real speech in all three languages** |
| Rule-based red-flag screening | Built; deliberately simple, **not clinical triage** |
| Anatomy images | In use for the prototype; **I'm still confirming the licence for the source image** (see `CREDITS.md`) |
| Medical content (parts, pain types, wording) | A first draft; **not yet reviewed by a physiotherapist or physician** |
| Accessibility | Designed for it, automatically smoke-tested; **no formal audit and no testing with disabled users yet** |
| Real users | **None yet.** There is no usage data to report |
| Clinical validation | **None.** Nothing here has been evaluated for accuracy or outcomes |
| Regulatory review | **Not done.** Software that offers AI-assisted health guidance may be regulated in some countries; I need proper advice on this before wider release |

## Where I want to take it

The prototype proves the interaction. The harder, more valuable work is the content and the evidence behind it. Roughly in this order:

1. **Clinical review.** Have physiotherapists review every body part, pain type, warning sign and piece of wording, record who reviewed what and when, and mark each item as draft, reviewed or approved.
2. **A proper terminology layer.** Move the vocabulary out of the code into a structured data set where every term has a stable ID and four kinds of names: the formal term, the everyday name, how patients actually say it, and a plain-language explanation, in each language. This also helps the voice feature map "my shoulder blade hurts when I lift" to the right structure. I want to base it on recognised standards where licensing allows, and I haven't settled which.
3. **Better translation.** Native review of Persian and French, with Canadian French vocabulary for Quebec users (for example, "physiothérapeute" rather than the term more common in France).
4. **More anatomy.** Muscles, joints and nerves, with an illustrated map that has been checked by an anatomist, and original or properly licensed artwork.
5. **Assessments and exercises.** Common pain and function questionnaires, and a reviewed exercise library with instructions, precautions and progressions, shown only after the safety screen.
6. **Accessibility audit** with real assistive-technology users, then fixes.
7. **Privacy and regulatory review**, including data-processing terms for the AI provider and a decision on how the voice feature should be offered in different countries.
8. **Pilot with real users** and a way to collect feedback, so decisions come from use rather than guesses.

## How support would help

Rough categories, in the order they matter most:

- Paying physiotherapists and translators for clinical and language review
- Licensed or commissioned anatomy illustrations
- An independent accessibility audit and user testing
- Legal and regulatory advice on health software and AI
- Hosting and AI usage costs for a pilot

I'm happy to share more detail on any of these.

## Under the hood

Deliberately simple.

- Static site on Cloudflare Pages, written in plain HTML, CSS and JavaScript with no framework
- One serverless function (`functions/api/analyze.js`) that calls Google's Gemini API; it checks the request origin, limits input size, wraps the user's text so it is treated as data, forces a structured response, and re-validates it before sending it back
- Strict Content-Security-Policy; the build script computes the script hash and minifies with esbuild
- Speech-to-text uses the browser's own speech recognition (so it works in Chrome, Edge and Safari, not Firefox), and the app falls back to typing
- Settings are kept in the browser's local storage

Repository layout:

```
public/       the website that gets served
functions/    the server-side function
source/       editable source, build script, security and accessibility notes
docs/         deployment guide
```

To host it yourself, follow `docs/DEPLOYMENT.md`.

## Contact

Iman Kiani · https://imankiani.com/

## Licence

Code: MIT (see `LICENSE`). Anatomy images: see `CREDITS.md`; they are not covered by the MIT licence.
