---
name: pg-payments
description: Use when building or reviewing payment-gateway / PG integrations, card payment flows, 결제·정산(settlement), refunds/cancellations, PCI-DSS scope, webhooks/callbacks, idempotency, or Korean PG 대행 work (워너페이먼츠, KG이니시스, 토스페이먼츠, 나이스페이, 카카오페이, 네이버페이). Covers 카드결제, 빌링키, 환불, 정산, PG 어댑터.
---

# PG / Card Payments (결제 대행)

## Overview
Payment integration is mostly about **not losing money and not leaking cards**. The two failure modes that cost real money: (1) double-charging / lost charges from missing idempotency, (2) trusting a webhook that wasn't actually from the PG. Everything else is plumbing. Reuse a proven PG-adapter pattern before writing new integration code.

## When to Use
- Integrating a Korean PG (KG이니시스, 토스페이먼츠, 나이스페이, 워너페이먼츠, 카카오페이, 네이버페이) or building 결제대행/정산
- Card payment (승인/취소/부분취소), 빌링키(recurring), 정기결제
- Reviewing money-path code for security/correctness
- Designing settlement (정산) / reconciliation (대사)

## PG Adapter Pattern (reuse this, don't reinvent)
Wrap every PG behind one interface so business logic never touches vendor specifics:
```
interface PgAdapter {
  approve(orderId, amount, token): Promise<PgResult>   // 승인
  cancel(txId, amount?, reason): Promise<PgResult>      // 취소/부분취소
  issueBillingKey(cardInfo): Promise<BillingKey>        // 빌링키 발급
  chargeBillingKey(key, amount): Promise<PgResult>      // 정기결제
  verifyWebhook(headers, rawBody): boolean              // 콜백 검증
}
```
Adding a PG = one new adapter, zero changes elsewhere.

## Non-negotiable rules
| Rule | Why |
|------|-----|
| **Idempotency key** on every approve/charge | Network retry must not double-charge. Key = orderId, dedup server-side. |
| **Verify webhook signature** on raw body | Anyone can POST your callback URL. Verify HMAC/signature before trusting amount. |
| **Amount check** server-side against your order | Never trust client-sent amount; compare PG-reported amount to your DB order. |
| **Never store PAN/CVV** | Store PG token / 빌링키 only. Storing card data = full PCI-DSS scope. |
| **Reconcile daily** (정산 대사) | Compare PG settlement file vs your ledger; flag mismatches. |
| **Log every state transition** | pending→approved→settled→cancelled, append-only, for dispute/감사. |

## PCI-DSS scope reduction
Use the PG's hosted payment window / 결제창 or tokenization so raw card numbers never touch your server (SAQ-A instead of SAQ-D). If you handle PAN directly you inherit the full audit burden — avoid.

## Settlement (정산)
- PG pays out net = 매출 − 수수료(fee) − 환불(refunds) on a T+n cycle.
- Store per-transaction fee at approval time (rates change).
- Daily job: pull PG 정산내역, match to internal txns, surface unmatched rows to a review queue. Never auto-write mismatches.

## Common Mistakes
- Trusting webhook/redirect params without signature verification → spoofed "paid" orders.
- No idempotency → retried request double-charges the customer.
- Confirming order on client redirect only (user closes tab = lost order) → always confirm via server webhook too.
- Storing full card data "for convenience" → catastrophic PCI/legal exposure.
- Float for money → use integer minor units (원 has no subunit, but still avoid float for fees/percentages).
- Hardcoding PG secret keys → env vars / secret manager, rotate on exposure.

## Korea specifics
- 카드 정기결제 = 빌링키(billing key) issuance then charge; PG-specific issuance flow.
- 전자금융거래법 / 여신전문금융업법 apply to 대행 businesses — 정산 주기·에스크로·PG 등록 요건 확인.
- 현금영수증·세금계산서 발행 연동이 흔한 추가 요구사항.
