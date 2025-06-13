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

## Common localization tools

You can use the common localization tools on applications that support languages that are supported by LocaleStation. The `LanguageCommon` class has the necessary functions to perform the following operations:

* `Language` (property): Sets and gets the current language for localized applications and libraries
* `GetInferredLanguage()`: Infers the languages and returns the first language from the list
* `GetInferredLanguages()`: Infers the languages according to the current UI culture
* `Translate()`: Translates a given string using the string ID, the language action type name, and the language
* `IsCustomActionDefined()`: Checks to see if the language action type is defined or not
* `AddCustomAction()`: Adds the language action
* `RemoveCustomAction()`: Removes the language action
* `GetAction()`: Gets the language action by name

{% hint style="info" %}
Here are some of the best design principles when creating applications and libraries:

* All applications that use Localizer for localization are advised to use the `Language` property (getter and setter), while all libraries should only use the value of `Language` to avoid wrong languages to be set.
* You can optionally infer the supported language from the current UI culture and set the above property to the resultant value.
* Adding language actions requires a definition of the `LanguageLocalActions` instance, which needs:
  * A function delegate to the `Languages` property (usually `() => LocalStrings.Languages`)
  * A function delegate to the `Localizations` property (usually `() => LocalStrings.Localizations`)
  * A function delegate to the `Translate()` function (usually `LocalStrings.Translate`)
  * A function delegate to the `Exists()` function (usually `LocalStrings.Exists`)
  * A function delegate to the `CheckCulture()` function (usually `LocalStrings.CheckCulture`)
  * A function delegate to the `ListLanguagesCulture()` function (usually `LocalStrings.ListLanguagesCulture`)
*   An example of defining the above actions class boils down to this:\


    ```csharp
    LanguageCommon.AddCustomAction("Demonstration", new(() => LocalStrings.Languages, () => LocalStrings.Localizations, LocalStrings.Translate, LocalStrings.CheckCulture, LocalStrings.ListLanguagesCulture, LocalStrings.Exists));
    ```
{% endhint %}

After that, in your application, you can make a function that checks for your actions type, adds it if necessary, and translates your string using your actions type to reduce repetition. The simplest example for this kind of translation is this (assuming that the `localType` constant is `LocalLocalization`:

```csharp
internal static class LanguageTools
{
    private const string localType = "LocalLocalization";

    internal static string GetLocalized(string id)
    {
        if (!LanguageCommon.IsCustomActionDefined(localType))
            LanguageCommon.AddCustomAction(localType, new(() => LocalStrings.Languages, () => LocalStrings.Localizations, LocalStrings.Translate, LocalStrings.CheckCulture, LocalStrings.ListLanguagesCulture, LocalStrings.Exists));
        return LanguageCommon.Translate(id, localType);
    }
}
```

Call the above function on the text that you want translated, passing the string ID as the only parameter. This setup respects the chosen language managed by LocaleStation. Applications can set the current language as follows:

```csharp
LanguageCommon.Language = "ger";
```

{% hint style="info" %}
If you've set the language to an unsupported language, you'll see the string placeholders as mentioned in the beginning of this page.
{% endhint %}
