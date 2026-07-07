---
name: crypto-stablecoin-card
description: Use when working on crypto / stablecoin debit cards (USDT/Tether, USDC, Visa/Mastercard), on-ramp / off-ramp, card issuing (BIN sponsor / issuer processor / program manager), custody, KYC/AML/Travel Rule, or web3 payment rails. Covers 테더 카드, 스테이블코인 결제, 카드 발급, 온·오프램프, VASP.
---

# Crypto / Stablecoin Cards (테더·Visa/Master 카드)

## Overview
A "crypto card" is a **normal Visa/Mastercard whose funding source is crypto, converted to fiat at authorization**. You almost never build the card rails yourself — you sit on top of an **issuer processor + BIN sponsor**, and your real work is the crypto side (custody, on/off-ramp, settlement) plus **KYC/AML compliance**, which is what actually gets these programs shut down. Compliance-first or not at all.

## When to Use
- Designing a USDT/USDC-funded card, 테더 카드, or web3 payment product
- Card issuing stack decisions, on/off-ramp integration, custody model
- KYC/AML/Travel Rule and Korean VASP(특금법) considerations

## The stack (you don't own most of it)
```
User crypto wallet
   │  (off-ramp: stablecoin → fiat at auth)
   ▼
Program Manager (you) ── Issuer Processor ── BIN Sponsor (licensed bank)
   │                                              │
   └────────── Visa / Mastercard network ─────────┘ → merchant
```
- **BIN sponsor / issuing bank** — holds the license, sponsors your card BIN.
- **Issuer processor** — authorizes/settles transactions (e.g. Marqeta-class).
- **Program manager** — you: UX, funding logic, crypto custody, compliance ops.
- **Card-as-a-service providers** for crypto specifically: Rain, Immersve, Baanx, Reap — they bundle sponsor+processor so you integrate one API.

## Funding models
| Model | How | Note |
|-------|-----|------|
| **Just-in-time (JIT) funding** | At auth, convert exactly enough stablecoin→fiat | Custody stays with user longer; needs fast off-ramp |
| **Pre-funded** | User tops up fiat balance from crypto beforehand | Simpler auth, you hold float |

## Custody
- **Custodial** (you hold keys) → simpler UX, you carry security + regulatory weight.
- **Non-custodial / MPC** (user retains control, MPC co-signs) → less liability, harder UX.
Choose deliberately; it drives your entire regulatory profile.

## Compliance (the part that kills programs)
- **KYC** at onboarding, **AML** transaction monitoring, sanctions/OFAC screening.
- **Travel Rule** — originator/beneficiary info on transfers above threshold between VASPs.
- **Korea:** 특금법 — 가상자산사업자(VASP) 신고 의무, 실명확인 입출금 계정, ISMS. 카드·환전 결합 시 여전법/전금법도 검토.
- Stablecoin issuer risk: USDT/USDC de-peg or freeze (Tether/Circle can freeze addresses) → design for it.

## Common Mistakes
- Building "the card" first, compliance later → programs get frozen; compliance is the product.
- Assuming stablecoin = risk-free → de-peg, issuer freeze, chain congestion are real.
- Ignoring off-ramp FX + network fees in unit economics → margin evaporates.
- Custodial custody without matching license/registration → regulatory exposure.
- No sanctions/AML screening → sponsor bank terminates you.
- Treating 특금법/VASP as optional for a Korea-facing product → illegal operation.
