# nuup-specification (*99#)

A public specification of the NUUP USSD Service by NPCI (**`*99#`**).

## How to read this specification

This is a USSD Menu specification and subject to change over time. Single Menu items can be dialed directly (Such as `*99*4*2*1#` to change the language to English). Most menu items require a session to be created, which requires manual input. These are denoted via the `->` symbol as sequential inputs.

## Top Level Menu

Selection|Description
---------|-----------
1|Send Money
2|Request Money
3|Check Balance
4|My Profile
5|Pending Requests
6|Transactions
7|UPI PIN

## 1.1 Send Money/To Mobile Number

Needs:

1. MOBILENUMBER: Mobile Number of the beneficiary with `*99#` service enabled and configured with a mobilenumber@upi VPA.
2. UPIPIN: Current UPI PIN
3. REMARKS: Remarks for the transaction. Set to `1` to disable.

- Dial `*99*1*1*MOBILENUMBER*AMOUNT*REMARKS` -> `UPIPIN` -> `2` to make transaction and exit.
- Dial `*99*1*1*MOBILENUMBER*AMOUNT*REMARKS` -> `UPIPIN` -> `1` to make transaction and save mobile number as beneficiary.

## 1.3 Send Money/To UPI ID

Needs:

1. VPA: UPI VPA of the beneficiary.
2. UPIPIN: Current UPI PIN
3. REMARKS: Remarks for the transaction. Set to `1` to disable.

- Dial `*99*1*3*VPA*AMOUNT*REMARKS` -> `UPIPIN` -> `2` to make transaction and exit.
- Dial `*99*1*3*VPA*AMOUNT*REMARKS` -> `UPIPIN` -> `1` to make transaction and save mobile number as beneficiary.

## 1.4 Send Money/Saved Beneficiary

Needs:

1. ID: Beneficiary ID
2. REMARKS: Remarks for the transaction. Set to `1` to disable.

- Dial `*99*1*4*ID*AMOUNT*1*REMARKS` -> `UPIPIN` to make transaction and exit.

## 1.5 Send Money/To IFSC+Bank Account

- Dial `*99*1*5#` -> `IFSC` -> `AccountNumber` -> `REMARKS` -> `UPIPIN`

## 2 Request Money (Raise Collect Request)

- Dial `*99*2*MOBILE*AMOUNT*REMARK*1#`
- Dial `*99*2*VPA*AMOUNT*REMARK*1#`

## 3 Check Balance

Needs:

1. Current UPI PIN

- Dial `*99*3#` -> `UPIPIN`

## 4.1 Change Bank Account

- Dial `*99*4*1#` -> `BANK` -> `LAST06 MMYY` -> `NEWPIN` -> `NEWPIN` to change Bank Account and reset PIN
- Dial `*99*4*1#` -> `BANK` -> `LAST06 MMYY` -> `NEWPIN` -> `NEWPIN` -> `NEWPIN` to change Bank Account, reset PIN, and fetch balance.

## 4.2 Change Language

- Dial `*99*4*2*LANG#`.

Popular Language Codes:

1. English
2. Hindi

## 4.3 My Details

No response.

## 4.4 UPI ID

- Dial `*99*4*4*1*ID#` to add a new UPI ID

ID = 1-3, to pick from a pre-generated list of 3 IDs.

## 4.5 Manage Beneficiary

## 4.6 I am a Merchant

## 4.7 De Register

## 6.1 Transactions/Recent Transactions

Dial `*99*6*1#` to get a list of recent transactions.

## 6.2 Transactions/Rewards 

Dial `*99*6*2# to get a list of recent rewards.

## 7.1 Set/Forgot UPI PIN

Needs: 

1. `LAST06` - Last 6 digits of the Debit Card associated with the bank account.
2. `MMYY` - Expiry Date of the Debit Card associated with the bank account.
3. `UPIPIN` - 4 or 6 digit UPI PIN to be set.

- Dial `*99*7*1#` -> `LAST06 MMYY` -> `UPIPIN` -> `UPIPIN` -> `UPIPIN` to reset PIN and check balance
- Dial `*99*7*1#` -> `LAST06 MMYY` -> `UPIPIN` -> `UPIPIN` to only reset the PIN

## 7.2 Change UPI PIN

Needs:

1. `OLDPIN` - Current UPI PIN
2. `NEWPIN` - New UPI PIN to be set

- Dial `*99*7*2#` -> `OLDPIN` -> `NEWPIN` -> `NEWPIN`

## References

- See https://www.npci.org.in/what-we-do/99/product-overview for public documentation of the service features
- https://www.npci.org.in/what-we-do/99/live-members for list of supported ISPs, languages, and banks. 
- Notably, *Jio is not a supported provider*. 
- Also see the [NPCI FAQ](https://www.npci.org.in/what-we-do/99/faqs) regarding the same.

## Implementation Notes

- Note that Android does not natively support multi-session USSD Codes.
- You can automate USSD sessions in Android by using the Accessibility service.
- iOS [does not support dialing USSD codes programatically](https://developer.apple.com/library/ios/featuredarticles/iPhoneURLScheme_Reference/PhoneLinks/PhoneLinks.html#//apple_ref/doc/uid/TP40007899-CH6-SW1).
