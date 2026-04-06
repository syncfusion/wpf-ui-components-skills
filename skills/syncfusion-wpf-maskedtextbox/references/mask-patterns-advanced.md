# Mask Patterns and Input Restriction — Advanced Patterns

## Date and Time Patterns

### Time (24-hour format)

```xml
<syncfusion:SfMaskedEdit 
    Mask="(0[0-9]|1[0-9]|2[0-3]):[0-5][0-9]"
    MaskType="RegEx"/>
```
**Accepts:** 00:00, 13:45, 23:59  
**Format:** HH:MM (00-23:00-59)

### Date (MM/DD/YYYY)

```xml
<syncfusion:SfMaskedEdit 
    Mask="(0[1-9]|1[0-2])/(0[1-9]|[12][0-9]|3[01])/\d{4}"
    MaskType="RegEx"/>
```
**Accepts:** 01/15/2024, 12/31/2023  
**Format:** MM/DD/YYYY with validation

## Product and ID Patterns

### Product Key (XXXXX-XXXXX-XXXXX)

```xml
<syncfusion:SfMaskedEdit 
    Mask="[A-Z\d]{5}-[A-Z\d]{5}-[A-Z\d]{5}-[A-Z\d]{5}-[A-Z\d]{5}"
    MaskType="RegEx"/>
```
**Accepts:** AB12C-3D45E-6F78G-9H01I-2J34K  
**Format:** Five groups of 5 alphanumeric characters

### Credit Card Number

```xml
<syncfusion:SfMaskedEdit 
    Mask="\d{4} \d{4} \d{4} \d{4}"
    MaskType="RegEx"/>
```
**Accepts:** 1234 5678 9012 3456  
**Format:** #### #### #### ####

### Zip Code (US)

```xml
<syncfusion:SfMaskedEdit Mask="\d{5}-\d{4}" MaskType="RegEx"/>
```
**Accepts:** 12345-6789  
**Format:** #####-####

### Bank Account Number (8-12 digits)

```xml
<syncfusion:SfMaskedEdit Mask="\d{8,12}" MaskType="RegEx"/>
```
**Accepts:** 12345678, 123456789012  
**Length:** 8 to 12 digits

## Technical Patterns

### Hexadecimal Number

```xml
<syncfusion:SfMaskedEdit 
    Mask="\\x[0-9A-Fa-f]{1,2}"
    MaskType="RegEx"/>
```
**Accepts:** \x0A, \xFF, \x1  
**Format:** \x followed by 1-2 hex digits

### Hexadecimal Color Code

```xml
<syncfusion:SfMaskedEdit 
    Mask="# [A-Fa-f0-9]{6}"
    MaskType="RegEx"/>
```
**Accepts:** #FF5733, #00aaff  
**Format:** # followed by 6 hex digits

### IP Address (Simple)

```xml
<syncfusion:SfMaskedEdit 
    Mask="\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}"
    MaskType="RegEx"/>
```
**Accepts:** 192.168.1.1, 10.0.0.1  
**Note:** Doesn't validate 0-255 range

## Input Validation Modes (reference)

See `mask-patterns-input-restriction.md` for primary guidance on `ValidationMode`. This file documents advanced patterns beyond the common numeric and communication masks.
