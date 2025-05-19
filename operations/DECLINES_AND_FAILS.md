# Webhook Responses and Status Codes

## Overview

This document describes the webhook responses and status codes used in the system for card operations, including card issuance and recharge operations.

## HTTP Response Codes

- All webhook responses return HTTP status code **200** (OK) if the request was processed successfully, regardless of the operation result.
- The actual operation status is reflected in the response body.

## Status Codes

### Card Record Status Codes

| Status Code | Description |
|------------|-------------|
| 0 | PENDING - Operation is in progress |
| 1 | SUCCESS - Operation completed successfully |
| 4 | DECLINED/FAILED - Operation was declined or failed |
| 5 | FAILED - Operation failed |
| 7 | UNKNOWN_ERROR - Operation status is unknown |

### Webhook Response Format

#### Successful Response
```json
{
    "success": true,
    "code": 0,
    "message": "success"
}
```

#### Failed Response
```json
{
    "success": false,
    "code": 3,
    "message": "failed [error details]"
}
```

## Card Operations

### Card Issuance (/checkIfIssued)

#### Success Response
```json
{
    "code": 200,
    "status": "SUCCESS",
    "message": "Card issued successfully"
}
```

#### Failure Response
```json
{
    "code": 200,
    "status": "FAILED",
    "message": "Bank rejected card issuance"
}
```

or

```json
{
    "code": 200,
    "status": "AUDIT_NOT_PASS",
    "message": "KYC not passed"
}
```

### Card Recharge (/rechargeResult)

#### Success Response
```json
{
    "code": 200,
    "status": "SUCCESS",
    "message": "Recharge successful"
}
```

#### Failure Response
```json
{
    "code": 200,
    "status": "FAILED",
    "message": "Bank declined recharge"
}
```

## Webhook Notifications

### Card Status Change Webhook
```json
{
    "cardId": "[card_id]",
    "timestampUTC": "[timestamp]",
    "isActive": "[boolean]",
    "isClosed": "[boolean]",
    "isFrozen": "[boolean]"
}
```

### Recharge Webhook
```json
{
    "cardId": "[card_id]",
    "reason": "RECHARGE:[card_id]:PAID",
    "status": 1,
    "userId": "[user_id]",
    "recordId": "[record_id]",
    "recordKey": "RECHARGE::::[request_id]:1",
    "requestId": "[request_id]",
    "executedAt": "[timestamp]",
    "resultAmount": "[amount]",
    "operationType": "RECHARGE",
    "operationAmount": "[amount]"
}
```

## Important Notes

1. All webhook responses return HTTP 200, even for failed operations
2. The actual operation status is in the response body
3. For failed operations, check the `status` and `message` fields
4. Webhook notifications are sent asynchronously after the operation is processed
5. The system maintains logs of all webhook interactions for debugging purposes
