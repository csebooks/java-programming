---
title: 'ResourceBundle'
weight: 10
---


`ResourceBundle` (in `java.util`) is a mechanism for **internationalization (i18n)** that allows you to load locale-specific resources like text messages, labels, or configuration data from property files or classes.

---

### Scenarios

- Supporting multiple languages in your application.
- Separating locale-specific strings from code.
- Dynamically loading language resources based on user locale.

---

### Sample Usage

Suppose you have two property files:

- `Messages_en.properties`
- `Messages_fr.properties`

```properties
# Messages_en.properties
greeting=Hello
farewell=Goodbye

# Messages_fr.properties
greeting=Bonjour
farewell=Au revoir
```

Java code to load and use these resources:

```java
import java.util.Locale;
import java.util.ResourceBundle;

public class ResourceBundleExample {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("Messages", Locale.FRANCE);
        System.out.println(bundle.getString("greeting"));  // Output: Bonjour

        bundle = ResourceBundle.getBundle("Messages", Locale.US);
        System.out.println(bundle.getString("farewell"));  // Output: Goodbye
    }
}
```

---

### Notes

* Resource bundles can be backed by property files or Java classes.
* The `getBundle()` method chooses the best match for the requested locale.
* Missing keys throw `MissingResourceException` if not handled.

Using `ResourceBundle` helps create applications that are easy to adapt to different languages and regions.

