## Refund Status Codes and Responses

### Refund Status Codes

| Status Code | Description | When Used |
|------------|-------------|-----------|
| 8 | REFUNDED | - Refund completed successfully<br>- Funds returned to partner's balance |
| 4 | FAILED | - Refund operation failed<br>- Bank rejected the refund |
| 7 | UNKNOWN_ERROR | - Refund status is unknown<br>- System couldn't determine the final status |

### Refund Webhook Response

#### Success Response
```json
{
    "notifytype": "refund",
    "taskid": "[request_id]",
    "status": "success",
    "remarks": "Refund completed successfully",
    "cardid": "[card_id]",
    "amount": "[refund_amount]",
    "currency": "[currency]"
}
```

#### Failure Response
```json
{
    "notifytype": "refund",
    "taskid": "[request_id]",
    "status": "failed",
    "remarks": "[error_message]",
    "cardid": "[card_id]"
}
```

### Refund Process Flow

1. **Initiation**
    - Refund can be initiated in three scenarios:
        - Card closure with remaining balance
        - Failed operation requiring refund
        - Manual refund request

2. **Processing**
    - System checks the refund amount
    - Updates partner's balance:
        - Adds amount to `refundBalance`
        - Updates `available` balance
    - Creates refund record with status 8 (REFUNDED)

3. **Completion**
    - Webhook notification sent to partner
    - Partner can check refund status via API
    - Refund amount credited to partner's account

### Refund Status Check

**Endpoint:** `/direct/api/{partnerId}/{timestamp}/refundResult?requestId={requestId}`

**Success Response:**
```json
{
    "status": "SUCCESS",
    "refundAmount": "[amount]"
}
```

**Failure Response:**
```json
{
    "status": "FAILED",
    "message": "[error_message]"
}
```

### Important Notes

1. Refund status 8 (REFUNDED) is used for successful refunds
2. Failed refunds are retried automatically
3. Partner's balance is updated only after successful refund
4. All refund operations are tracked in the account records