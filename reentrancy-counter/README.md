# Reentrancy counter

## Description

This bot checks transactions for reentrancy. This is done by the bot watching the call stack in transaction traces. It then reports the number of recurrent calls with varying severity based on various thresholds.

## Supported Chains

- Ethereum
- Optimism
- BNB Smart Chain
- Polygon
- Fantom
- Arbitrum
- Avalanche

## Alerts

- NETHFORTA-25
  - Fired when multiple nested calls occur to the same contract in a transaction (Reentrancy)
  - It reports all possible severities based on different call volume thresholds.
  - Type is always set to "suspicious"
  - The metadata includes:
  - `address`: The contract address where the reentrancy occurred
  - `reentrancyCount`: A reentrancy counter based on how many times it occurred
  - `anomalyScore`: Score of how anomalous the alert is (0-1)
    - Score calculated by finding amount of either `NETHFORTA-25` alerts out of the total number of transactions that contained traces processed by this bot and differs based on chain.
  - Labels:
    - Label 1:
      - `entity`: The transaction's hash
      - `entityType`: The type of the entity, always set to "Transaction"
      - `label`: The type of the label, always set to "Reentrant contract call"
      - `confidence`: The confidence level of reentrancy happening in the transaction (0-1). Always set to `1`
    - Label 2:
      - `entity`: The reentered contract address
      - `entityType`: The type of the entity, always set to "Address"
      - `label`: The type of the label, always set to "Reentrancy Victim"
      - `confidence`^: The confidence level of the reentered contract being a victim (0-1).
        - Confidence level calculated based on the Finding Severity:
          - Info -> 0.3, Low -> 0.4, Medium -> 0.5, High -> 0.6, Critical -> 0.7
    - Label 3:
      - `entity`: The address of the initiator of the transaction
      - `entityType`: The type of the entity, always set to "Address"
      - `label`: The type of the label, always set to "Reentrancy Attacker"
      - `confidence`^: The confidence level of the transaction initiator being an attacker (0-1).
        - Confidence level calculated based on the Finding Severity:
          - Info -> 0.3, Low -> 0.4, Medium -> 0.5, High -> 0.6, Critical -> 0.7
