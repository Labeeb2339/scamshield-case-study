# Architecture Notes

## Input adapters

Each scanner converts its input into a common analysis request. Text can come directly from a pasted message, local OCR, a QR payload, URL fields, a suspicious filename, or caller-number metadata. Input adapters are kept separate from risk scoring so they can be permission-tested independently.

## Hybrid detector

The detector combines three evidence families:

1. a compact on-device text classifier for general scam probability and category hints;
2. deterministic rules for urgency, credential requests, payment instructions, impersonation, typosquatting, shorteners, punycode, suspicious file extensions, and Malaysian scam language;
3. optional reputation data for phone numbers or bank accounts.

The fusion layer uses diminishing returns so repeated weak signals do not grow without bound. High-risk combinations still escalate. Each contributing signal carries a human-readable explanation.

## Android boundary

Flutter owns the main UI, local state, analysis services, and result models. Native Android services are required for notification access and call-screening roles. These capabilities are permission-gated and may be restricted by the device manufacturer or Android version.

The current call feature screens caller numbers. Capturing and transcribing the live carrier conversation is a separate platform, privacy, and product problem and is not part of the implemented claim.

## Evaluation boundary

Unit and regression tests can verify deterministic scoring behaviour, known scam patterns, and protected legitimate-message cases. They cannot establish real-world sensitivity, specificity, demographic fairness, or resistance to future scam tactics. Those require a documented held-out dataset and user study.
