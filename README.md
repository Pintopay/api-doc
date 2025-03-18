# Corp-API Integration Guide

This README provides detailed information on how to integrate and interact with the Corp-API using the provided `integration-test` application. Each endpoint's functionality, flow, and implementation details are described to help you align with the Corp-API requirements effectively.

---

## Key Concepts

- **Swagger**: [API Documentation](https://on.pintopay.me/direct/swagger-ui/index.html)
- **EndpointName**: The string after the last slash in the URL.
- **NONCE**: A unique string used as one of the unique identifiers for a request.
- **Timestamp**: Must not differ from the server time by more than 20 seconds for the request to be valid.
- **Private Key**: RSA key with a recommended size of 2048.
    - Used to sign requests to Corp-API.
    - Used to decrypt card details.
- **Signature**: An RSA-encrypted string combining `partnerId`, `timestamp`, `nonce`, and `endpointName` in the format:
  ```
  [partnerId]:[timestamp]:[nonce]:[endpointName]
  ```
- **SHA256 with RSA**: The signature must be generated using the SHA256 hashing algorithm combined with RSA encryption.
- **How to generate keys** [read here](https://www.webdevsplanet.com/post/how-to-generate-rsa-private-and-public-keys?expand_article=1)
- **Public Key (DER Format)**: The public key should be provided in DER format for compatibility with Corp-API.
  ```
  readFileContent(file).replaceAll("-----[\\sA-Z]+ KEY-----", "").replace("\n", "").trim() 
  ```
- **Base64 Signature**: The generated signature must be encoded in Base64 before being sent in the request.
- **URL Encoding**: The partnerId should be URL-encoded before being used in API requests.

---

### Environments

#### Sandbox

- Trigger partner account creation by sending your pre-generated public key.
- Trigger ***test*** methods to create test partners.
- **Base URL**: `https://on.pintopay.me/direct`

#### Production

- Provide your pre-generated public key.
- A new partner will be registered for you, and a unique partner ID will be returned.
- **Base URL**: `https://with.pintopay.me/direct`
- **Mirror URL**: `https://my.pintopay.me/direct`

---

## Partner Model

### Partner Info Retrieval

Fetch partner details via the endpoint:

```
GET /direct/api/{partnerId}/{timestamp}/info
```

| Property                       | Type    | Format | Description                                                                |
| ------------------------------ | ------- | ------ | -------------------------------------------------------------------------- |
| **partnerId**                  | string  | uuid   | Unique partner ID                                                          |
| **name**                       | string  |        | Partner's name                                                             |
| **paymentAddress**             | string  |        | Tron address                                                               |
| **availableCardTypes**         | array   | string | Available card types                                                       |
| **webhook\_for\_applications** | string  |        | Webhook URL for obtaining results on card applications                     |
| **webhook\_for\_recharges**    | string  |        | Webhook URL for obtaining results on card recharge                         |
| **webhook\_for\_transactions** | string  |        | Webhook URL for receiving new card transactions                            |
| **webhook\_for\_3ds**          | string  |        | Webhook URL for receiving 3DS codes                                        |
| **lastBalanceUpdate**          | integer | int64  | Last balance operation timestamp in milliseconds                           |
| **balance**                    | number  |        | Account balance                                                            |
| **refundBalance**              | number  |        | Refund operations balance (e.g., closing the card with a positive account) |
| **available**                  | number  |        | Full available amount (balance + refundBalance)                            |

---

### Recharge Partner Account

#### Commons

- **Tron Transfer**: Use the specified address to top up the account.

#### Sandbox

- **Network**: Shasta network (**USDC** required).
- **Endpoint**: `https://on.pintopay.me/test/top-up`

#### Production

- **Network**: Mainnet (**USDT**, differs from sandbox).

---

## Webhooks

The platform supports the following webhook types:

- **Webhook for Recharges**: Updates on card recharge operations.
- **Webhook for Transactions**: Notifications about new card transactions.
- **Webhook for 3DS Codes**: Delivery of 3DS codes for enhanced security.

### Defining Webhooks

Partners can define webhooks by making `POST` requests to the respective endpoints:

1. **Set Webhook for Applications**

    ```
    POST /direct/api/{partnerId}/{timestamp}/webhookForApplications
    ```

2. **Set Webhook for Recharges**

   ```
   POST /direct/api/{partnerId}/{timestamp}/webhookForRecharges
   ```

3. **Set Webhook for Transactions**

   ```
   POST /direct/api/{partnerId}/{timestamp}/webhookForTransactions
   ```

4. **Set Webhook for 3DS Codes**

   ```
   POST /direct/api/{partnerId}/{timestamp}/webhookFor3DS
   ```
   
5. **Set Webhook for Card Status Update**

   ```
   POST /direct/api/{partnerId}/{timestamp}/webhookForCardStatusUpdate
   ```

### Required Parameters

| Parameter     | Description                                              |
| ------------- | -------------------------------------------------------- |
| **partnerId** | Unique identifier for the partner.                       |
| **timestamp** | Timestamp of the request for security purposes.          |
| **nonce**     | Unique value for each request to prevent replay attacks. |
| **signature** | Cryptographic signature to validate the request.         |
| **url**       | Endpoint URL where the webhook events will be sent.      |

---

## Endpoints

### 1. `/create-and-activate-card`

**Method**: GET\
**Description**: Facilitates the creation and activation of a card through a multi-step process.

**Flow**:

1. **Server Time Retrieval**: Fetch the current server time using `GET /direct/api/time`.
2. **Partner Account Creation**: (Test only) Create a partner account using `POST /direct/test/newAccount`.
3. **Partner Info Retrieval**: Fetch details via `GET /direct/api/{partnerId}/{timestamp}/info`.
4. **User Registration**: Register a user using `POST /direct/api/{partnerId}/{timestamp}/registerUser`.
- One user for multiple card types
5. **Card Opening**: Open a card using `POST /direct/api/{partnerId}/{timestamp}/openCard`.

    **or** Open a card using KYC for VISA cards `POST /direct/api/{partnerId}/{timestamp}/openCardKyc`.

**CHECK THIS** ->    2025_03_17_MB_VIRTUAL_VISA_DEBIT_USD_GREEN - KYC Form Requirements.md

6. **Card Activation and Recharge**:
    - Confirm card issuance via `GET /direct/api/{partnerId}/{timestamp}/checkIfIssued`.
    - Recharge the card using `POST /direct/api/{partnerId}/{timestamp}/recharge`.
7. **Final State Logging**: Log the partner and card details.

**Implementation Guidelines**:

- Use robust error handling.
- Generate signatures as per Corp-API guidelines.
- Monitor responses for debugging purposes.

**Service Interaction**: `TestApiFlowService.createAndActivateCard()` orchestrates this flow.\
**External API Calls**: `getTime()`, `createNewToppedAccount()`, `registerUser()`, `openCard()`, `checkIfIssued()`, `recharge()`, `rechargeResult()`.

---

### 2. `/recharge-card`

**Method**: GET\
**Description**: Tests the recharge flow for a card.

**Flow**:

1. Set up partner, user, and card via `executePartnerUserCardFlow()`.
2. Initiate a recharge using `POST /direct/api/{partnerId}/{timestamp}/recharge`.
3. Confirm recharge success using `GET /direct/api/{partnerId}/{timestamp}/rechargeResult`.
4. Log the recharge amount and partner state.

**Implementation Guidelines**:

- Ensure consistent signature calculations.
- Retry failed requests for transient errors.

**Service Interaction**: `TestApiFlowService.testCardRechargeFlow()` manages this flow.\
**External API Calls**: `recharge()`, `rechargeResult()`.

---

### 3. `/account-records`

**Method**: GET\
**Description**: Retrieves account-related records for a partner.

**Flow**:

1. Set up partner and user via `executePartnerUserCardFlow()`.
2. Fetch records using `GET /direct/api/{partnerId}/{timestamp}/accountRecords`.

**Implementation Guidelines**:

- Use pagination for large datasets.
- Apply filters for efficient responses.

**Service Interaction**: `TestApiFlowService.getAccountRecords()` handles this flow.\
**External API Calls**: `getAccountRecords()`.

---

### 4. `/card-details`

**Method**: GET\
**Description**: Fetches detailed information about a specific card.

**Flow**:

1. Initialize partner, user, and card using `executePartnerUserCardFlow()`.
2. Fetch details via `GET /direct/api/{partnerId}/{timestamp}/card`.
3. Retrieve balance using `GET /direct/api/{partnerId}/{timestamp}/cardBalance`.
4. Log card details and transaction history.

**Implementation Guidelines**:

- Decrypt sensitive details.
- Securely handle card information.

**Service Interaction**: `TestApiFlowService.getCardDetails()` manages this flow.\
**External API Calls**: `getCard()`, `getCardBalance()`, `getCardRecords()`.

---

### 5. `/freeze-unfreeze-close-card`

**Method**: GET\
**Description**: Manages card lifecycle states (freeze, unfreeze, close).

**Flow**:

1. Initialize partner, user, and card via `executePartnerUserCardFlow()`.
2. **Freeze Card**:
    - Freeze using `POST /direct/api/{partnerId}/{timestamp}/freezeCard`.
    - Confirm state via `GET /direct/api/{partnerId}/{timestamp}/freezeRequestResult`.
3. **Unfreeze Card**:
    - Unfreeze using `POST /direct/api/{partnerId}/{timestamp}/unfreezeCard`.
    - Confirm state via `GET /direct/api/{partnerId}/{timestamp}/freezeRequestResult`.
4. **Close Card**:
    - Close using `POST /direct/api/{partnerId}/{timestamp}/closeCard`.
    - Confirm closure via `GET /direct/api/{partnerId}/{timestamp}/closeCardResult`.

**Implementation Guidelines**:

- Validate state transitions before proceeding.
- Log all operations for auditing purposes.

**Service Interaction**: `TestApiFlowService.freezeUnfreezeAndCloseCard()` manages this flow.\
**External API Calls**: `freezeCard()`, `freezeRequestResult()`, `unfreezeCard()`, `closeCard()`, `closeCardResult()`.

---

## Notes

- Ensure time synchronization to meet the 20-second validity window.
- Use secure methods for signature generation and validation.

