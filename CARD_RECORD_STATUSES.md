# Explanation of Statuses in CardRecord

## Overview

- `transactionStatus`— a numeric status from the `status` field in the `CardRecord` entity, reflects the technical state of the transaction.
- `status` — a string field `transactionStatus` in `CardRecord`, describes the textual state of the transaction (e.g., `SUCCESS`, `FAILED`).
- `type` — the type of the operation, such as `PENDING`, `DECLINED`, `ACCEPTED`, and may influence the value of `status`.

---

## Details

### 1.2 `transactionStatus` (from the `CardRecord.status` field)

A numeric field representing the transaction state:

| Value | Meaning              |
|-------|----------------------|
| -1    | Revoked              |
| 0     | Pending              |
| 1     | Placed               |
| 2     | Success              |
| 3     | Completed            |
| 4     | Failed               |
| 5     | Expired              |

---

### 1.3 `status` (string field `transactionStatus`)

This is a textual status string, typically coming from the provider or set manually. Possible values include:

- `SUCCESS`
- `FAILED`
- `DECLINED`
- `AUTHORIZED`
- `PENDING`
- etc.

---

### 1.1 `type` (transaction type)

Defines the type of operation. Possible values:

- `PURCHASE`
- `ATM`
- `AUTH`
- `TOPUP`
- `FEE`
- `CONSUME`
- `PENDING`
- `DECLINED`
- `ACCEPTED`



# Объяснение статусов в CardRecord

## В общих чертах

- `transactionStatus` — числовой статус из поля `status` в сущности `CardRecord`, отражает техническое состояние транзакции.
- `status` — строковое поле `transactionStatus` из `CardRecord`, описывает текстовое состояние транзакции (например, `SUCCESS`, `FAILED`).
- `type` — тип операции, например, `PENDING`, `DECLINED`, `ACCEPTED`, и может влиять на определение `status`.

---

## Подробности

### 1.2 `transactionStatus` (из поля `CardRecord.status`)

Числовое поле, обозначающее состояние транзакции:

| Значение | Значение по смыслу        |
|----------|---------------------------|
| -1       | Отменена (`revoked`)       |
| 0        | В ожидании (`pending`)     |
| 1        | Размещена (`placed`)       |
| 2        | Успешна (`success`)        |
| 3        | Завершена (`completed`)    |
| 4        | Провалена (`failed`)       |
| 5        | Истёк срок (`expired`)     |

---

### 1.3 `status` (строковое поле `transactionStatus`)

Это строковый текстовый статус, приходящий из провайдера или задаваемый вручную. Возможные значения:

- `SUCCESS`
- `FAILED`
- `DECLINED`
- `AUTHORIZED`
- `PENDING`
- и т.п.

---

### 1.1 `type` (тип транзакции)

Определяет тип операции. Возможные значения:

- `PURCHASE`
- `ATM`
- `AUTH`
- `TOPUP`
- `FEE`
- `CONSUME`
- `PENDING`
- `DECLINED`
- `ACCEPTED`
