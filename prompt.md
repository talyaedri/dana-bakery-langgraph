# Dana’s Bakery — Codex implementation prompt

Build a Google Colab Python notebook implementing a bakery order workflow with LangGraph. Use deterministic Python only, no LLM calls and no API key. Include installation, explanations, a graph rendered using draw_mermaid_png(), and executable tests.

## Input
Support graph.invoke({"item": "babka", "quantity": 2, "day": "Thursday", "city": "Jerusalem"}).

## Catalog
Prices: challah 18, babka 45, rugelach 30 per box, sourdough 25.
Initial stock: challah 10, babka 0, rugelach 3, sourdough 4.
Croissants are not sold.

## Rules
- Reject unknown products and quantities that are not positive integers (exclude booleans).
- Require stock for the entire requested quantity.
- If stock is insufficient, check successors in babka -> rugelach -> sourdough -> challah. Check at most two alternatives. Accept the first sufficient alternative; reject if both fail or the chain ends. Keep the quantity and use the replacement price.
- Apply a 15% discount to goods subtotals strictly above 150, except on Friday. Never discount delivery.
- Empty city means free pickup. Jerusalem delivery costs 20. Other cities switch to free pickup with an explanation.
- Round final payment to two decimal places.
- Generate warm deterministic template messages explaining substitutions, pickup changes, or rejection.
- Record every executed node in ordered steps, including alternative attempts.

## Graph and state
Use explicit state for input, original item, alternative counter, pricing, outcome, and steps. Create nodes for validation, stock check, next alternative, goods pricing, fulfillment/final total, rejection, and response. Use conditional edges, including the alternative retry loop.

## Output
Every result contains status (accepted/rejected), item (actual product for accepted orders), delivery (pickup/delivery), total, message, and steps.
For rejected orders, total is 0, item is the original requested item, and delivery is pickup as a structural default; message must state that no order was placed.

## Explicit assumptions
Each invocation uses the same initial stock independently without decrementing stock across invocations. Document this assumption and the rejected-output convention.

## Verification
Include at least six distinct example orders and meaningful assertions covering every decision route: direct acceptance, first/second alternative success, two failed alternatives, exhausted chain, unknown product, zero/negative/non-integer quantity, eligible discount, Friday exclusion, exactly-150 threshold, Jerusalem delivery, empty-city pickup, and out-of-area conversion. Cases can overlap. Check all six required keys on every result. Notebook must run top-to-bottom in a fresh Colab session.

## Revision log
Implemented explicit day/city validation (English weekday, string city), case/whitespace normalization for day/city, Decimal money calculations, and 18 graph cases. The exactly-150 boundary is tested directly at the pricing node because initial inventory cannot produce that subtotal. Graph drawing requires network access to the rendering service.
