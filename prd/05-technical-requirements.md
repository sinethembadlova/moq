# Technical Requirements

## Overview

MOQ handles real money, regulated financial flows, and sensitive business identity data. The technical requirements below reflect those obligations. They are not exhaustive implementation specifications — those belong in blueprints — but they establish the non-negotiable constraints any technical design must satisfy.

## Payment Infrastructure

The platform uses Paystack as its payment provider for wallet funding and disbursements. All payment flows — buyer deposits into their wallet, pledge transfers from a buyer's wallet to a campaign wallet, supplier payouts on campaign success, and refunds to buyer wallets on campaign failure — must be routed through Paystack's APIs. Payment handling must be consistent with Paystack's supported settlement flows and any applicable card scheme requirements. No payment data (card numbers, bank account details) may be stored directly on the platform's infrastructure.

## Campaign Wallet Infrastructure

Each campaign has its own ring-fenced virtual wallet that holds pledged funds on behalf of participants until the campaign is resolved. Campaign wallets must be isolated from each other and from the platform's operating funds — pledges collected for one campaign cannot be applied to another or used for platform expenses. On campaign success, the campaign wallet disburses to the supplier minus the platform's commission. On campaign failure, it refunds each participant's pledge in full to their buyer wallet. Fund state transitions must be atomic — a disbursement or refund either completes fully or does not occur, with no partial states.

## KYC and Regulatory Compliance

Both buyers and suppliers must complete KYC verification before participating. The platform is likely subject to the Financial Intelligence Centre Act (FICA) given that it pools and moves funds on behalf of third parties, and to the Protection of Personal Information Act (POPIA) given the volume of personal and business identity data it collects. Legal guidance on the precise regulatory obligations — particularly around whether the platform constitutes a financial services provider or payment system operator — must be obtained before launch. KYC processes must be designed to satisfy the applicable obligations once confirmed.

## Data Security

All personally identifiable information (PII) and business identity data collected during KYC must be encrypted at rest and in transit. Access to sensitive data must be role-restricted and auditable. The platform must maintain logs sufficient to satisfy any regulatory audit or dispute resolution requirement. Data must be stored in a jurisdiction consistent with POPIA requirements.

## Availability and Performance

Campaign deadlines are time-critical. A buyer who cannot pledge before a deadline closes loses their opportunity, and a campaign that closes without settling correctly erodes trust. The platform must maintain high availability during active campaign windows. Wallet balances, pledge states, and campaign progress must be consistent and accurate in real time — eventual consistency is not acceptable for financial state.