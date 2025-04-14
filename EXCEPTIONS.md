# API Error Reference

This document outlines the standard API error codes and their corresponding meanings returned by the system. Use it to identify, understand, and resolve issues when integrating with the API.

---

## 401 Unauthorized

**Description:** The `timestamp` is either missing, too old, or too far in the future.  

---

## 402 Payment Required

**Description:** Insufficient funds to perform the operation.  

---

## 403 Forbidden

**Default Description:** Access denied.  

---

## 405 Signature Required

**Description:** The request is missing a valid `signature`, or the `nonce` has already been used.

---

## 406 Incorrect Parameter

**General Description:** One or more request parameters are invalid.

| Returned Message                                | Explanation |
|-------------------------------------------------|-------------|
| `invalid partnerId`                             | The `partnerId` is corrupted or incorrectly encoded in the URL. |
| `invalid cardId`                                | `cardId` must be a valid UUID in a URL-safe format. |
| `invalid UID format`                            | The string could not be parsed into a UUID. |
| `invalid requestId`                             | The `requestId` is not a valid UUID. |
| `invalid cardType`                              | `cardType` is not supported. |
| `invalid statuses parameter: a,b -> b`          | The `statuses` parameter includes non-numeric values. |
| `invalid types parameter: a,b -> b`             | The `types` parameter includes unsupported values. |

**Resolution:** Check all request parameters for correctness and proper formatting.

---

## 409 Already Exists

**Description:** The object already exists, or the operation was already performed.  

---

## 410 All Attempts Failed

**Description:** The operation failed after all retries.

---

## 425 Awaiting

**Description:** The operation has not completed yet.



---

---

---



# Справочник ошибок API

Данный документ описывает стандартные коды ошибок API и их значения. Используйте его для определения и устранения проблем при интеграции с системой.

---

## 401 Unauthorized (Неавторизовано)

**Описание:** Параметр `timestamp` отсутствует, устарел или слишком опережает текущее время.

---

## 402 Payment Required (Требуется оплата)

**Описание:** Недостаточно средств для выполнения операции.

---

## 403 Forbidden (Запрещено)

**Описание по умолчанию:** Доступ запрещён.

---

## 405 Signature Required (Требуется подпись)

**Описание:** Запрос не содержит допустимой подписи `signature`, либо `nonce` уже использовался.

---

## 406 Incorrect Parameter (Некорректный параметр)

**Общее описание:** Один или несколько параметров запроса недопустимы.

| Сообщение `ApiResponse`                         | Объяснение |
|-------------------------------------------------|------------|
| `invalid partnerId`                             | `partnerId` повреждён или некорректно закодирован в URL. |
| `invalid cardId`                                | `cardId` должен быть валидным UUID в URL-совместимом формате. |
| `invalid UID format`                            | Строка не может быть интерпретирована как UUID. |
| `invalid requestId`                             | `requestId` не является корректным UUID. |
| `invalid cardType`                              | Указан неподдерживаемый тип карты `cardType`. |
| `invalid statuses parameter: a,b -> b`          | Параметр `statuses` содержит недопустимые (нечисловые) значения. |
| `invalid types parameter: a,b -> b`             | Параметр `types` содержит неподдерживаемые значения. |

**Решение:** Проверьте корректность всех параметров запроса.

---

## 409 Already Exists (Уже существует)

**Описание:** Объект уже существует или операция уже была выполнена ранее.

---

## 410 All Attempts Failed (Все попытки завершились неудачей)

**Описание:** Операция не удалась после всех попыток выполнения.

---

## 425 Awaiting (Ожидание)

**Описание:** Операция ещё не завершена.  
**Сообщение:** `not ready`