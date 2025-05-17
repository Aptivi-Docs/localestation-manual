---
description: How can I access localized strings?
icon: earth-americas
---

# Localization Tools

An application that is being built with the LocaleStation analyzer can access the `LocalStrings` class that contains the following properties:

* `Languages`: The languages that have been processed during the generation time. It lists all supported languages in a single assembly.
* `Localizations`: A list of localization IDs that have been fetched for every language in a single assembly.

The following functions are dynamically generated to support AOT:

* `Translate()`: Gives a translated version of a string using a language and a localized text ID.
* `Exists()`: Checks to see if a translated version of a string using a language and a localized text ID exists or not.
* `CheckCulture()`: Checks to see if the given culture in a specific language exists.
* `ListLanguagesCulture()`: Lists languages in a given culture.

{% hint style="info" %}
You can use those functions to get localized strings, but you'll have to take the following points into consideration:

* If the language doesn't exist, then it returns a localized string in this format: `{lang}_{id}`
  * For example, if we don't have `brz` and `LOCAL_STRING`, you'll get `brz_LOCAL_STRING`.
* If the language exists, but the localization ID doesn't exist, it returns: `{id}`
  * For example, if we have `eng` but not `LOCAL_STRING`, you'll get `LOCAL_STRING`.
* Otherwise, it returns a localized string according to both the ID and the language.
  * For example, if we have `spa` and `TEXT_HI`, you'll get `¡Hola!`.
{% endhint %}
