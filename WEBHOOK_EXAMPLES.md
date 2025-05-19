## whApplications
- whApplications >> {"requestId":"tgAgpvSzXA3dFRBDAYPprUA","operationType":"APPLY","operationAmount":"45.00","userId":"GgAKht4ZdBf8BFFTFZLtFjg","cardId":"fCogjPwxVCfoFRVWVdL9qFA","cardType":"ART_VIRTUAL_VISA_DEBIT_USD","expiredAt":1740168857429,"status":"UNKNOWN"}
- whApplications >> {"recordId":"XCoAjN4Z3B2ZAEEDAMf9B8Q","userId":"GgAKht4ZdBf8BFFTFZLtFjg","executedAt":1740082447844,"status":4,"recordKey":"REGISTER_USER::GgAKht4ZdBf8BFFTFZLtFjg:4","operationType":"REGISTER_USER","operationAmount":"0.00","resultAmount":"0.00","reason":"REGISTER_USER:GgAKht4ZdBf8BFFTFZLtFjg:SUCCESS"}
- whApplications >> {"recordId":"EiqqDHQZ3BH5QUFSVML5Hyw","userId":"GgAKht4ZdBf8BFFTFZLtFjg","executedAt":1740082448094,"status":0,"recordKey":"APPLY::GgAKht4ZdBf8BFFTFZLtFjg:0","operationType":"APPLY","operationAmount":"0.00","resultAmount":"0.00","reason":"APPLY:GgAKht4ZdBf8BFFTFZLtFjg:CREATED"}
- whApplications >> {"recordId":"aKIiht4z1CV9BFRDQIetqQQ","userId":"GgAKht4ZdBf8BFFTFZLtFjg","cardId":"fCogjPwxVCfoFRVWVdL9qFA","executedAt":1740082457429,"status":1,"requestId":"tgAgpvSzXA3dFRBDAYPprUA","recordKey":"APPLY::::tgAgpvSzXA3dFRBDAYPprUA:1","operationType":"APPLY","operationAmount":"45.00","resultAmount":"955.00","reason":"APPLY:GgAKht4ZdBf8BFFTFZLtFjg:PAID"}
- whApplications >> {"recordId":"vqqiJn4ZdC3gVAFCEdO9rDQ","userId":"GgAKht4ZdBf8BFFTFZLtFjg","cardId":"fCogjPwxVCfoFRVWVdL9qFA","executedAt":1740082457432,"status":2,"requestId":"tgAgpvSzXA3dFRBDAYPprUA","recordKey":"APPLY::::tgAgpvSzXA3dFRBDAYPprUA:2","operationType":"APPLY","operationAmount":"45.00","resultAmount":"0.00","reason":"APPLY:GgAKht4ZdBf8BFFTFZLtFjg:PLACED"}

## Webhook Notifications

### Card Status Change Webhook
```json
{
    "cardId": "[card_id]",
    "timestampUTC": [timestamp],
    "isActive": [boolean],
    "isClosed": [boolean],
    "isFrozen": [boolean]
}
```

### Card Application Webhook
```json
{
    "status": "[status]",
    "message": "[message]",
    "updatedAt": [timestamp]
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
    "executedAt": [timestamp],
    "resultAmount": "[amount]",
    "operationType": "RECHARGE",
    "operationAmount": "[amount]"
}
```

### 3DS Code Webhook
```json
{
    "providerType": "[provider]",
    "providerCardId": "[card_id]",
    "createdAt": [timestamp],
    "code": "[3ds_code]"
}
```

### Transaction Webhook
```json
{
    "cardId": "[card_id]",
    "userId": "[user_id]",
    "partnerId": "[partner_id]",
    "recordId": "[record_id]",
    "recordKey": "[record_key]",
    "requestId": "[request_id]",
    "executedAt": [timestamp],
    "resultAmount": "[amount]",
    "operationType": "[operation_type]",
    "operationAmount": "[amount]",
    "status": [status_code]
}
```