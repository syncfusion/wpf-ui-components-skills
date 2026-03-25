---
layout: post
title: Culture and Formatting in WPF Double TextBox control | Syncfusion®
description: Learn about Culture and Formatting support in Syncfusion® WPF Double TextBox control, its elements and more.
platform: WPF
control: DoubleTextBox
documentation: ug
---

# Culture and Formatting in WPF Double TextBox

Value of `DoubleTextBox` can be formatted via `Culture`, `NumberFormat`, and dedicated properties like `NumberGroupSeparator`, `NumberGroupSizes`, `NumberDecimalDigits`, `NumberDecimalSeparator`.

## Culture based formatting
Set the `Culture` property to format decimal/group separators according to culture.

## NumberFormatInfo based formatting
Use `NumberFormat` to supply a `NumberFormatInfo` for fine-grained control.

## Formatting with dedicated properties
`NumberGroupSeparator`, `NumberGroupSizes`, `NumberDecimalDigits`, and `NumberDecimalSeparator` have higher priority when both used with `NumberFormat`.
