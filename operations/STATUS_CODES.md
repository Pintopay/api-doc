## Status Codes

### Card Record Status Codes

| Status Code | Description | When Used |
|------------|-------------|-----------|
| 0 | PENDING | - Operation is in progress<br>- Initial state of a transaction<br>- Waiting for bank processing |
| 1 | SUCCESS | - Operation completed successfully<br>- Card issued successfully<br>- Recharge completed successfully<br>- Transaction approved |
| 4 | DECLINED/FAILED | - Operation was declined by the bank<br>- Transaction failed<br>- Recharge was rejected<br>- Card issuance was rejected |
| 5 | FAILED | - Operation failed due to technical issues<br>- Bank rejected the operation<br>- System error occurred |
| 7 | UNKNOWN_ERROR | - Operation status is unknown<br>- Bank response was unclear<br>- System couldn't determine the final status |

### Provider Request Status Codes

| Status | Description | When Used |
|--------|-------------|-----------|
| RECHARGED | Operation completed successfully | - After successful recharge<br>- When funds are credited to the card |
| FAILED | Operation failed | - Bank rejected the operation<br>- Technical failure occurred |
| UNKNOWN_ERR | Unknown error occurred | - Bank response was unclear<br>- System couldn't determine the status |
| AUDIT_NOT_PASS | KYC verification failed | - When card issuance is rejected due to KYC<br>- When user verification fails |
| CLOSED_BEFORE | Card was closed before operation | - When trying to operate on a closed card<br>- When card was closed during processing |
| NOT_READY | Operation is not ready | - Initial state of a request<br>- Waiting for bank processing |

### Transaction Status Codes

| Status | Description | When Used |
|--------|-------------|-----------|
| PENDING | Transaction is processing | - Initial state of a transaction<br>- Waiting for bank processing |
| SUCCESS | Transaction completed successfully | - Transaction approved by bank<br>- Operation completed successfully |
| DECLINED | Transaction was declined | - Bank rejected the transaction<br>- Transaction failed |
| FAILED | Transaction failed | - Technical failure occurred<br>- System error during processing |

### Webhook Response Status Codes

| Status | Description | When Used |
|--------|-------------|-----------|
| 0 | Success | - Webhook processed successfully<br>- Operation completed successfully |
| 1 | Processing Error | - Error during webhook processing<br>- Temporary failure |
| 3 | Failed | - Permanent failure<br>- Critical error occurred |

### Card Status Flags

| Flag | Description | When Used |
|------|-------------|-----------|
| isActive | Card is active | - Card is ready for use<br>- All verifications passed |
| isClosed | Card is closed | - Card was closed by user or bank<br>- Card is no longer usable |
| isFrozen | Card is frozen | - Card is temporarily blocked<br>- Suspicious activity detected |

### Important Notes

1. Status codes are used consistently across different operations (issuance, recharge, transactions)
2. The system maintains a log of all status changes for auditing purposes
3. Status changes are notified via webhooks to the partner system
4. Some status codes may have different meanings depending on the operation type
5. The system automatically retries failed operations where appropriate
6. All status changes are logged with timestamps for tracking and debugging