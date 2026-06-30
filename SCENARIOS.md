# dmtool — example multi-step sessions

<!-- GENERATED — do not edit by hand; regenerate with scripts/gen-scenarios.sh. -->

Realistic, **multi-turn** sessions an author (or a coding agent) drives `dmtool` through — each a sequence of plain-language asks, *in order*, that build up or evolve a document model. They show the *kind* of non-trivial, multi-step work the CLI is exercised with: from-scratch authoring, evolving an existing model's rules, refactors, computations, dates and ranges, multi-file workspaces, and more.

These are the **asks**, not worked transcripts. For step-by-step sessions **with the actual `dmtool` commands and their output** — re-executed and exact-matched in CI — see the demos in [`examples/`](examples/README.md) (e.g. [`cli-edit-loop`](examples/cli-edit-loop.md), [`cli-apply`](examples/cli-apply.md), [`cli-structure-edit`](examples/cli-structure-edit.md)).

## task 01, prompted in German

1. Erstelle ein Dokumentmodell für eine Person mit einer Privatadresse und einer Geschäftsadresse, füge passende Bedingungen hinzu. Und stelle sicher, dass die Person mindestens 18 Jahre alt ist.
2. Verschiebe die Felder der Adressgruppe in ein separates Dokumentmodell und binde es dann für die Privat- und die Geschäftsadresse ein.

## person + date-of-birth → age≥18

1. create a document model for a person: their full name, date of birth, and the date they joined. Add validation that the person is at least 18 years old (based on their date of birth), and that the join date is not in the future.
2. Add a renewal date field, and a rule that the renewal must be within 2 years after the join date.

## a registration form's field constraints

1. create a member registration model: a member with a name, an email, a country, and a membership tier (basic, plus, premium). Make the obviously-required ones required.
2. the member id must look like M followed by six digits.
3. when the member id format is wrong, show the message in both English and German.
4. the display name must be between 2 and 50 characters.
5. a referral code is optional in general, but required when the tier is premium.

## subscription billing computation

1. Create a document model for a software subscription. It has a tier, a billing section with a base monthly fee and an effective monthly fee, and a repeatable list of add-ons, each with a name and a monthly fee. The effective monthly fee is calculated as twice the base fee.
2. Change the effective-fee calculation so it is twice the base fee plus the total of all the add-ons' monthly fees.

## a multi-model workspace

1. create a shared catalog model: a product with a sku, a name, and a price. Then create a separate order model with a customer and an order date.
2. in the order model, include the catalog so each order carries the product details.
3. both models' money amounts should use one shared "Money" type defined once.
4. add a rule on the order: when the order is placed, the product's sku must be filled.
5. rename the order's customer field to "buyer" everywhere.

## a full, realistic authoring session

1. create a document model for a person with a home and work address, add proper conditions. And ensure the person's age is >= 18.
2. add a German locale to the person model.
3. create some users and roles and attach them to the model.
4. change the repeatability of the work address to 10, and somehow flag the main work address.
5. move the fields of the address group to a separate dm and include it for both home and work.

## evolve an EXISTING model's rules

1. each line item must have a count greater than zero.
2. no two line items may have the same SKU.
3. the backorder quantity should be required. — *(then, as a follow-up once turn 3a is done:)* actually, only require it when express shipping is on.
4. when the order priority is EXPRESS or OVERNIGHT, every line item must have a SKU.
5. rename the line-item SKU field to ProductSku everywhere.

## price an insurance policy

1. create a policy model with a policyholder and a repeatable list of coverages, each with a type and a premium amount. The total premium is the sum of all coverage premiums.
2. the applied discount is the loyalty rate when the holder is a member, otherwise the seasonal promo rate, otherwise nothing.
3. the total of the liability-type coverage premiums must not exceed the policy's liability cap.
4. premiums are euro amounts with two decimals, and the discount is a percentage between 0 and 100.
5. the net premium is the total premium minus the discount.

## a venue booking

1. create a venue booking model: a venue, and a repeatable list of bookings, each with a booking period (a date range) and a daily start time.
2. a booking can't start in the past — its period must not begin before today.
3. no two bookings may have overlapping periods.
4. the daily start time must be between 9 in the morning and 5 in the afternoon.
5. a booking longer than 30 days needs manager approval.

## refactor a live model

1. move the backorder quantity field out to sit directly under the order, not wherever it is now.
2. actually, add a per-line-item "line note" field inside the line items.
3. remove the delivery date field — we don't track it anymore.
4. rename the line-item Count field to "Sku" to match the spec.
5. move the whole Items group so it sits inside the shipping address.

## type definitions + attachment & multiselect groups

1. create a support-case model: a case with a customer name and a priority. Define one shared "Money amount" type (a euro amount, 2 decimals) and use it for both an estimated cost and an approved cost.
2. the money amounts should allow 3 decimals now — change it in one place.
3. let the customer attach supporting documents to the case.
4. add a place to pick one or more affected products (a multi-select).
5. the approved cost must not exceed the estimated cost.
6. show me the money type and the two new groups, so I can confirm they're right.

## rule messages, severities, and token interpolation

1. each line item's count must be greater than zero — and the error must show the entered count in its text.
2. when there are several items, the message should say which line number is wrong.
3. add a rule: the backorder quantity shouldn't exceed 1000 — but make it a warning, not a hard error.
4. add an informational note when express priority is chosen (just info, no blocking).
5. the count message should also mention that orders over $1000 need approval — with a literal dollar sign.
6. all of these messages should also read correctly in German.

## validating a document with a COMPUTED field

1. In the subscription model, the effective fee must always be present.
2. Here's a subscription to check: the base fee is 49.90 and nothing else is filled in. Is it valid? Explain.
3. Now check the *stored data as-is*, without running the model's computations. Does that change the answer, and why?
