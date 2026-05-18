# PostBillPay Stored Card Flows

This folder contains sequence diagrams for PostBillPay payment flows.

## Option 1 — No Card Stored

A customer enters a new credit/debit card, selects to save details in PostBillPay, the payment is processed by FZ Processing, and the returned alias is stored in SP dB against Billpay Code `8881` and Merchant ID `ZZZ`.

## Rendered Diagram

![Stored Card Flow - Option 1](./stored-card-option1.svg)

## Mermaid Source

```mermaid
sequenceDiagram
    autonumber

    participant Customer as Customer01
    participant PBP as PostBillPay
    participant SPDB as SP dB
    participant Paynow as Paynow iFrame
    participant FZ as FZ Processing

    Note over Customer,PBP: Billpay Code = 8881
    Note over SPDB: Merchant ID = ZZZ

    Customer->>PBP: Choose Billpay code (8881)
    PBP->>SPDB: Determine Merchant ID from Billpay code
    SPDB-->>PBP: Merchant ID = ZZZ

    Customer->>PBP: Choose payment method: New credit/debit card

    PBP->>Paynow: Initialise iFrame
    Note right of Paynow: Payment + 3DS + Alias + Merchant ID

    Paynow-->>Customer: Render Paynow iFrame

    Customer->>Paynow: Enter card details
    Customer->>PBP: Select Save details and continue

    Paynow->>FZ: Process payment with 3DS + alias creation
    FZ-->>Paynow: Payment result + alias

    Paynow-->>PBP: Return payment result + alias

    PBP->>SPDB: Store alias against Billpay Code 8881 and Merchant ID ZZZ
    SPDB-->>PBP: Stored card available

    PBP-->>Customer: Payment complete
```

## Source Files

- Mermaid source: [`stored-card-option1.mmd`](./stored-card-option1.mmd)
- Rendered SVG: [`stored-card-option1.svg`](./stored-card-option1.svg)
