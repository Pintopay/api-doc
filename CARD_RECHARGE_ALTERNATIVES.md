You are ****not required**** to allow users to top up cards using all these methods.

---

**DEFAULT:**  

You can collect funds from users on your side, top up the partner balance manually, and call the recharge command yourself.

Pls take a look at GENERAL_DESCRIPTION.md:182 "/recharge-card"

---

**TRON:**  
Each card is now assigned a unique TRON wallet address.

If a user sends USDT to this address:
- If the amount is less than the minimum top-up amount via this method (`minTronTopUp`), the funds minus the `minTronTopUpCommission` will be transferred to the partner account’s refund balance.
- If the amount is greater than or equal to `minTronTopUp`, the card will be recharged with the sent amount minus the `rechargeCommission` percentage. However, the actual commission amount must be at least equal to `minTronTopUpCommission`.
- If the top-up fails for any reason, the funds minus the `minTronTopUpCommission` will be transferred to the partner account’s refund balance.

In all cases, a log entry will be created with type `ACCOUNT_RECHARGE`, `relatedTransactionType = "TRON"`, and the corresponding `cardId`. This is followed by the actual card recharge operation and then, if needed, a refund to the refund balance.

Each step triggers a separate webhook.

---

**TON:**  
Top-ups via TON in USDT have also been added. This method has a slightly different flow: all users are assigned a single shared TON address (`tonAddress`) and a unique `memo` (`tonMemo`) that must be included in the transaction. If the memo is missing, we won’t be able to accept the funds.

The logic follows the same steps as TRON with one key difference: there is no minimum commission for TON, only the `minTonTopUp`.

If USDT is received via TON:
- If the amount is less than `minTonTopUp`, the full amount will be transferred to the partner account’s refund balance.
- If the amount is greater than or equal to `minTonTopUp`, the card will be recharged with the sent amount minus the `rechargeCommission` percentage.
- If the top-up fails for any reason, the funds minus the `minTronTopUpCommission` will be transferred to the partner account’s refund balance.

As with TRON, a log entry will be created with type `ACCOUNT_RECHARGE`, `relatedTransactionType = "TON"`, and the `cardId`, followed by the recharge operation and potential refund.

Each step triggers a separate webhook.
