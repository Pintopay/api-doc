# MB_VIRTUAL_VISA_DEBIT_USD_GREEN - KYC Form Requirements

## Dear Partners,

To issue a new type of card, you must submit a KYC form containing the following required fields:

| Field | Field Code | Data Type | Additional Requirements |
|------|-----------|-----------|--------------------------|
| First Name | `first_name` | String | - |
| Middle Name | `middle_name` | String | - |
| Last Name | `last_name` | String | - |
| Email | `email` | String | Email format |
| Mobile Country Code | `mobile_code` | Number | - |
| Mobile Number | `mobile` | Number | 5 to 15 digits |
| Date of Birth | `birthday` | Date | Format YYYY-MM-DD |
| Country Code | `country_id` | String | ISO 3166-1 alpha-3 (3 characters) |
| State/Province (Billing Address) | `state` | String | - |
| City (Billing Address) | `city` | String | - |
| Address (Billing Address) | `address` | String | - |
| Postal Code (Billing Address) | `zip_code` | String | 4 to 10 characters |

### Important Information:
- All fields are mandatory.
- If any field does not meet the specified requirements, the card issuance request will be rejected.
- Please ensure that the submitted data complies with these requirements to avoid processing delays.

If you have any questions, please contact us. We will be happy to assist you.

