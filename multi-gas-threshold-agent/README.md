# High Gas Used bot.

## Description

This bot detects transactions with unusual amount of gas used. It checks every transaction one by one and evalutes each of them by `MEDIUM_GAS_THRESHOLD` and `HIGH_GAS_THRESHOLD`.

## Supported Chains

- Ethereum

## Alerts

- NETHFORTA-1
  - Fired when a transaction uses more gas than the defined thresholds.
  - Severity:
    - For gas usage over `MEDIUM_GAS_THRESHOLD` is always set to "Medium".
    - For gas usage over `HIGH_GAS_THRESHOLD` is always set to "High".
  - Type is always set to "Suspicious".
  - Metadata includes:
    - `gas`: The amount of gas used by the transaction.
    - `anomalyScore`: Score of how anomalous the alert is (0-1)
      - Score calculated by finding amount of either `MEDIUM_GAS_THRESHOLD` or `HIGH_GAS_THRESHOLD` alerts out of the total number of alerts emitted by this bot.
  - Label
    - `entityType`: The type of the entity, always set to "Transaction"
    - `entity`: The transaction's hash
    - `label`: The type of the label, always set to "High Gas Transaction"
    - `confidence`: The confidence level of the address being an attacker (0-1). Always set to `1`