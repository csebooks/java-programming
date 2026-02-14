---
title: 'Currency'
weight: 7
---


The `Currency` class in `java.util` represents a specific **currency**, identified by an ISO 4217 currency code (like USD, INR, EUR). It provides details such as the symbol, code, and default number of fraction digits.

---

### Scenarios

- Displaying currency values in localized formats.
- Performing financial calculations involving currency codes.
- Integrating international payment systems.

---

### Sample Usage

```java
import java.util.Currency;
import java.util.Locale;

public class CurrencyExample {
    public static void main(String[] args) {
        // Get Currency by locale
        Currency usd = Currency.getInstance(Locale.US);
        System.out.println("Code: " + usd.getCurrencyCode());      // USD
        System.out.println("Symbol: " + usd.getSymbol());          // $
        System.out.println("Default fraction digits: " + usd.getDefaultFractionDigits()); // 2

        // Currency by code
        Currency inr = Currency.getInstance("INR");
        System.out.println("INR Symbol: " + inr.getSymbol());
    }
}
```

### Note

To format values with currency automatically, use it with `NumberFormat`:

```java
import java.text.NumberFormat;

double amount = 1500.75;
NumberFormat formatter = NumberFormat.getCurrencyInstance(Locale.UK);
System.out.println(formatter.format(amount));  // £1,500.75
```

`Currency` is key when writing internationalized applications that handle financial data.

