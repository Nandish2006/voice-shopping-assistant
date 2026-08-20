# Approach Write-up (200 words)

I built a single-page voice shopping assistant using the browser's Web Speech API for
speech-to-text, avoiding paid or heavy ML infrastructure. Transcribed commands are sent to
Groq's free LLM API (`openai/gpt-oss-20b`), which returns structured JSON — action, item,
quantity, and category — so the app handles natural variation like "I want to buy bananas" and
"add 2 bottles of milk" through one code path, instead of brittle regex rules per phrasing.

To keep the app resilient, I added a local rule-based parser as a fallback: if no API key is set
or the LLM call fails, commands still parse via simple pattern matching, so add/remove never
breaks. After adding an item, a second lightweight LLM call suggests a common substitute (e.g.,
soy milk for milk), shown as a dismissible chip.

Given the 8-hour budget, I prioritized the core loop — voice input, NLP parsing, quantity
handling, categorization, and one smart-suggestion feature — over covering every listed
requirement. Multilingual support, purchase-history recommendations, and price/brand-filtered
voice search were scoped out and documented as future work.

The UI is a single HTML file with no build step, deployed to Vercel for fast, dependency-free
hosting.

