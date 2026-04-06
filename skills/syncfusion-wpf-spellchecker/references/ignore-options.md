# Ignore Options

Configure SfSpellChecker to ignore specific types of text patterns during spell checking. This is useful for technical documents, emails, HTML content, and specialized text formats.

## Overview

SfSpellChecker provides six ignore options to skip common text patterns that shouldn't be flagged as spelling errors:

| Property | Ignores | Example |
|----------|---------|---------|
| `IgnoreEmailAddress` | Email addresses | john@example.com |
| `IgnoreUrl` | Internet addresses | https://example.com |
| `IgnoreHtmlTags` | HTML markup | `<html></html>` |
| `IgnoreMixedCaseWords` | Mixed case words | AbCDeFH, CamelCase |
| `IgnoreUpperCaseWords` | All uppercase words | NASA, HTTP, API |
| `IgnoreAlphaNumericWords` | Words with numbers | v2.0, A1B2C3, win32 |

**Default:** All ignore options are `false` by default.

## IgnoreEmailAddress

Prevents email addresses from being flagged as errors.

### Usage

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    IgnoreEmailAddress="True"/>
```

```csharp
spellChecker.IgnoreEmailAddress = true;
```

### When to Use

- Email client applications
- Contact forms
- Communication documents
- Support ticket systems
- Any text containing email addresses

### Example

**Without IgnoreEmailAddress:**
```
Text: "Contact support@example.com for help"
Error: "support" flagged (part of support@example.com)
```

**With IgnoreEmailAddress:**
```
Text: "Contact support@example.com for help"
No Error: Email address completely ignored
```

## IgnoreUrl

Prevents internet addresses and URLs from being flagged as errors.

### Usage

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    IgnoreUrl="True"/>
```

```csharp
spellChecker.IgnoreUrl = true;
```

### When to Use

- Documentation with links
- Web content editing
- Technical documents
- Reference materials
- Blog posts and articles

### Supported URL Formats

- HTTP/HTTPS: `https://www.example.com`
- FTP: `ftp://files.example.com`
- With paths: `https://example.com/path/to/page`
- With parameters: `https://example.com?param=value`
- Subdomains: `https://subdomain.example.com`

### Example

```csharp
spellChecker.IgnoreUrl = true;

// These URLs won't be flagged:
// https://help.syncfusion.com
// http://www.example.com/documentation
// ftp://files.company.com/downloads
```

## IgnoreHtmlTags

Prevents HTML tags and markup from being flagged as errors.

### Usage

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    IgnoreHtmlTags="True"/>
```

```csharp
spellChecker.IgnoreHtmlTags = true;
```

### When to Use

- HTML editors
- Web content management
- Email templates (HTML)
- Blog post editors
- WYSIWYG editors

### Example

**Without IgnoreHtmlTags:**
```
Text: "<html><body><div>Content</div></body></html>"
Errors: "html", "body", "div" all flagged
```

**With IgnoreHtmlTags:**
```
Text: "<html><body><div>Content</div></body></html>"
Only checks: "Content"
Ignores: All HTML tags
```

## IgnoreMixedCaseWords

Prevents words with mixed uppercase and lowercase letters from being flagged.

### Usage

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    IgnoreMixedCaseWords="True"/>
```

```csharp
spellChecker.IgnoreMixedCaseWords = true;
```

### When to Use

- Programming documentation (camelCase, PascalCase)
- Technical specifications
- Product names (iPhone, PowerPoint)
- Brand names
- Code comments

### Examples of Mixed Case Words

- `camelCase` - Programming convention
- `PascalCase` - Class names
- `iPhone` - Product name
- `JavaScript` - Language name
- `AbCDeFH` - Random mixed case

### Example

```csharp
spellChecker.IgnoreMixedCaseWords = true;

// These won't be flagged:
// myVariableName
// CalculateTotal
// iPhone
// PowerPoint
```

## IgnoreUpperCaseWords

Prevents all-uppercase words from being flagged as errors.

### Usage

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    IgnoreUpperCaseWords="True"/>
```

```csharp
spellChecker.IgnoreUpperCaseWords = true;
```

### When to Use

- Technical documents with acronyms
- API documentation
- Government/legal documents
- Scientific papers
- Military documents

### Common Uppercase Patterns

- Acronyms: `NASA`, `FBI`, `HTTP`, `API`
- Constants: `MAX_VALUE`, `DEFAULT_TIMEOUT`
- Emphasis: `IMPORTANT`, `WARNING`
- Codes: `SKU`, `ISBN`, `VIN`

### Example

```csharp
spellChecker.IgnoreUpperCaseWords = true;

// These won't be flagged:
// NASA, HTTP, API, JSON, XML
// WARNING, ERROR, INFO
// SKU123 (if also using IgnoreAlphaNumericWords)
```

## IgnoreAlphaNumericWords

Prevents words containing both letters and numbers from being flagged.

### Usage

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    IgnoreAlphaNumericWords="True"/>
```

```csharp
spellChecker.IgnoreAlphaNumericWords = true;
```

### When to Use

- Technical documentation
- Software version numbers
- Product codes
- Model numbers
- Serial numbers
- License keys

### Examples of Alphanumeric Words

- Version numbers: `v2.0`, `v1.5.3`
- Product codes: `SKU123`, `MODEL-X1`
- Variables: `var1`, `item2`
- Mixed formats: `A1B2C3`, `Win32Api`

### Example

```csharp
spellChecker.IgnoreAlphaNumericWords = true;

// These won't be flagged:
// v2.0, v1.5.3
// Win32, Win64
// SKU123, MODEL-X1
// A*&%#9ACe&981
```

## Combining Multiple Ignore Options

You can enable multiple ignore options simultaneously:

```xml
<syncfusion:SfSpellChecker 
    x:Name="spellChecker"
    EnableSpellCheck="True"
    IgnoreUrl="True"
    IgnoreEmailAddress="True"
    IgnoreHtmlTags="True"
    IgnoreMixedCaseWords="True"
    IgnoreUpperCaseWords="True"
    IgnoreAlphaNumericWords="True"/>
```

```csharp
// Enable all ignore options
spellChecker.IgnoreUrl = true;
spellChecker.IgnoreEmailAddress = true;
spellChecker.IgnoreHtmlTags = true;
spellChecker.IgnoreMixedCaseWords = true;
spellChecker.IgnoreUpperCaseWords = true;
spellChecker.IgnoreAlphaNumericWords = true;
```

## Common Configuration Patterns

### Pattern 1: Email Editor

```csharp
// Ideal for email composition
spellChecker.IgnoreEmailAddress = true;
spellChecker.IgnoreUrl = true;
```

### Pattern 2: HTML Editor

```csharp
// Ideal for HTML/web content
spellChecker.IgnoreHtmlTags = true;
spellChecker.IgnoreUrl = true;
spellChecker.IgnoreEmailAddress = true;
```

### Pattern 3: Technical Documentation

```csharp
// Ideal for code documentation
spellChecker.IgnoreMixedCaseWords = true;
spellChecker.IgnoreUpperCaseWords = true;
spellChecker.IgnoreAlphaNumericWords = true;
spellChecker.IgnoreUrl = true;
```

### Pattern 4: General Business Document

```csharp
// Minimal ignoring for standard text
spellChecker.IgnoreEmailAddress = true;
spellChecker.IgnoreUrl = true;
// Keep other options false for thorough checking
```

### Pattern 5: Programming IDE

```csharp
// Ideal for code comments/strings
spellChecker.IgnoreMixedCaseWords = true; // camelCase, PascalCase
spellChecker.IgnoreUpperCaseWords = true; // CONSTANTS, APIs
spellChecker.IgnoreAlphaNumericWords = true; // var1, item2
```

## Best Practices

### 1. Enable Only What You Need
Don't enable all options by default. Each option reduces spell check coverage.

```csharp
// Bad: Too permissive
spellChecker.IgnoreMixedCaseWords = true;
spellChecker.IgnoreUpperCaseWords = true;
spellChecker.IgnoreAlphaNumericWords = true;
// For a simple letter - too many false negatives

// Good: Targeted for use case
spellChecker.IgnoreEmailAddress = true; // Only what's needed
```

### 2. Consider Your Content Type
Match ignore options to your document type:

- **Simple text:** Minimal ignore options
- **Technical docs:** More permissive
- **Code:** Very permissive

### 3. User Preferences
Allow users to configure ignore options:

```csharp
// Save user preferences
Settings.Default.IgnoreUrls = spellChecker.IgnoreUrl;
Settings.Default.IgnoreEmails = spellChecker.IgnoreEmailAddress;

// Restore preferences
spellChecker.IgnoreUrl = Settings.Default.IgnoreUrls;
spellChecker.IgnoreEmailAddress = Settings.Default.IgnoreEmails;
```

### 4. Dynamic Configuration
Adjust options based on content:

```csharp
// Detect if text contains HTML
bool containsHtml = text.Contains("<") && text.Contains(">");
spellChecker.IgnoreHtmlTags = containsHtml;
```

## Troubleshooting

### Too Many False Positives

**Issue:** Valid words flagged as errors

**Solution:** Enable appropriate ignore options:
```csharp
// If technical terms flagged
spellChecker.IgnoreUpperCaseWords = true;
spellChecker.IgnoreMixedCaseWords = true;
```

### Too Few Errors Detected

**Issue:** Real errors not being caught

**Solution:** Review and disable unnecessary ignore options:
```csharp
// Be more selective
spellChecker.IgnoreAlphaNumericWords = false; // May be too broad
```
