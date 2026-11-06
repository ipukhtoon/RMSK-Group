# Payments and Ledger

## Core rule

**Payment status and service activation status are separate state machines.** A successful payment event does not automatically prove that the subscriber's service has been activated.

## Supported payment patterns

- Online gateway payments, when a gateway is contracted and integrated.
- Direct payment to the ISP using its approved instructions.
- Cash received and recorded by authorized ISP staff.
- Other local payment methods only after separate review and approval.

## Processing requirements

1. Create a server-side payment intent/reference.
2. Associate it with the correct tenant, subscriber and invoice/account.
3. Receive gateway/provider confirmation through a verified server-side channel.
4. Validate signature, amount, currency, merchant/account, reference and status.
5. Apply idempotency to prevent duplicate posting.
6. Record immutable transaction/audit details.
7. Reconcile gateway settlement and ISP ledger.
8. Trigger a separately authorized service action only after policy checks.

## Ledger principles

- Maintain traceable records for charges, receipts, refunds, reversals and adjustments.
- Never silently overwrite financial history.
- Corrections use linked reversal/adjustment entries.
- Record who performed manual cash entry and when.
- Keep platform SaaS subscription billing separate from ISP subscriber collections.
- Define currency, rounding, taxes, fees and settlement rules before implementation.

## Failure handling

Handle pending, failed, expired, reversed, refunded, disputed and duplicate events. Delayed callbacks and out-of-order events must not corrupt balances. Provide reconciliation and exception queues.

## Open decisions

Gateway providers, supported payment methods, settlement workflow, fee allocation, refund policy, tax treatment and accounting export formats require Pakistan-specific commercial/legal review and owner approval.
