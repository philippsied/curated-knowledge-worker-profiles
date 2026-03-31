<overview>
Stable reference for payment integration verification, subscription lifecycle,
and revenue infrastructure. Covers Stripe-based patterns but applies to any
payment provider. AI economics benchmarks are in `metrics-2026.md`.
</overview>

<payment_integration>

**Account setup checklist:**
- Business details verified (legal name, address, tax ID)
- Bank account connected and verified
- Statement descriptor set (what customers see on statements)
- Support email and phone configured
- Branding assets uploaded (logo, icon, colors)

**API configuration:**
- Live API keys generated (not test keys)
- Webhook signing secret stored securely
- API version pinned (do not use rolling latest)
- Error handling implemented for all API calls

**Required webhooks for subscriptions:**
```
customer.subscription.created
customer.subscription.updated
customer.subscription.deleted
invoice.payment_succeeded
invoice.payment_failed
checkout.session.completed
```

For each webhook: endpoint URL configured, signing secret verified,
idempotency handled (duplicate events), failure alerting configured.
</payment_integration>

<subscription_lifecycle>

**Test transactions (run in live mode with real cards):**
- Successful card payment
- 3D Secure authentication flow
- Subscription creation
- Subscription upgrade/downgrade
- Subscription cancellation
- Failed payment retry flow
- Refund processing

**Billing portal:**
- Customer portal link accessible
- Customers can update payment method
- Customers can view invoice history
- Customers can cancel/downgrade (if allowed)
- Proration settings configured correctly
</subscription_lifecycle>

<tax_and_fraud>

**Tax compliance:**
- Tax engine enabled (Stripe Tax or equivalent)
- Tax registration numbers collected
- Invoice includes required tax information
- Tax rates configured per region

**Fraud prevention:**
- Radar rules configured (or equivalent)
- Block lists set up if needed
- 3D Secure required for high-risk transactions
- Velocity limits configured
</tax_and_fraud>

<common_mistakes>

**Webhook failures:**
- Not verifying webhook signatures
- Not handling duplicate events
- No retry logic for failed processing
- Not logging webhook payloads for debugging

**Subscription issues:**
- Not handling failed payments gracefully
- No dunning emails configured
- Immediate cancellation vs. end of period confusion
- Missing proration handling

**Security issues:**
- API keys in client-side code
- Not using HTTPS for webhooks
- Storing full card numbers (use tokens)
- Missing PCI compliance considerations
</common_mistakes>

<go_live_verification>

Before announcing launch:
1. Process a real transaction with your own card
2. Verify webhook delivery in payment dashboard
3. Check customer portal works end-to-end
4. Test subscription lifecycle (create, upgrade, cancel)
5. Verify invoices include correct business details
6. Check statement descriptor appears correctly

**Post-launch monitoring alerts:**
- Payment failure rate > 5%
- Webhook delivery failures
- Unusual transaction patterns
- Chargeback notifications
- Subscription churn spikes
</go_live_verification>

<margin_protection>

**Usage caps (guardrails):**
- Monthly token/request limits by tier
- Rate limiting (requests per minute)
- Feature-specific caps
- Overage pricing (not free unlimited)

**Tier design for AI products:**
- Free: Limited AI requests (e.g. 100/month), basic models only
- Pro: Higher limits (e.g. 1000/month), better models, priority processing
- Enterprise: Custom limits, dedicated capacity option, SLA on response times
</margin_protection>
