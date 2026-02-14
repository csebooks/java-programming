---
title: 'Locale'
weight: 5
---


`Locale` is a class in `java.util` that represents a **specific geographical, political, or cultural region**. It is commonly used for **localization**—adapting software for different languages and countries.

---

### Scenarios

- Displaying messages, dates, and numbers in a region-specific format.
- Supporting multilingual applications.
- Selecting language-sensitive content (e.g., currency, units, greetings).

---

### Sample Usage

```java
import java.util.Locale;

public class LocaleExample {
    public static void main(String[] args) {
        // Default locale
        Locale defaultLocale = Locale.getDefault();
        System.out.println("Default: " + defaultLocale);

        // Create a locale for France (French language)
        Locale french = new Locale("fr", "FR");
        System.out.println("Display Language: " + french.getDisplayLanguage());
        System.out.println("Display Country: " + french.getDisplayCountry());

        // Predefined constants
        Locale us = Locale.US;
        Locale germany = Locale.GERMANY;

        System.out.println("US Locale: " + us);
        System.out.println("Germany Locale: " + germany);
    }
}

```

Locale is widely used with classes like DateFormat, NumberFormat, ResourceBundle, and Formatter to create culturally aware applications.