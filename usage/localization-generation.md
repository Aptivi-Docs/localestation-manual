---
description: How does LocaleStation generate localization?
icon: language
---

# Localization Generation

LocaleStation uses JSON as the format that stores information about each language, including cultures and localized strings. You can define a new language easily in the format shown below. For example, here is one for English:

```json
{
    "lang": "eng",
    "name": "English",
    "cultures": [ "en-US", "en-GB" ],
    "locs": [
        {
            "loc": "TEXT_HELLO_WORLD",
            "text": "Hello world!"
        },
        {
            "loc": "TEXT_HI",
            "text": "Hi!"
        }
    ]
}
```

This language file contains the necessary parameters to allow LocaleStation to generate classes related to each language. The following information is stored:

* `lang`: Contains a three-letter ID for a language, such as `eng`, `spa`, `ind`, etc.
* `name`: Contains a full name for a language, such as English, Spanish, Hindi, etc.
* `cultures`: Contains a list of valid cultures by their IDs, such as `en-US`, `sp-AR`, `hi-IN`, etc.
* `locs`: An array of locale information objects that consist of:
  * `loc`: Localization ID (must match but not necessarily exist for every language)
  * `text`: Final localized text to return

Once you're done defining new localization files, you'll have to import them to your project files as additional files. For example, if you've stored language files under `Resources/Languages`, you'll have to add the below item to the project:

```xml
<ItemGroup>
  <AdditionalFiles Include="Resources\Languages\*.json" AptLocIsLanguagePath="true" />
</ItemGroup>
```

{% hint style="info" %}
The `AptLocIsLanguagePath` variable is necessary, because it tells LocaleStation that the files specified, which are actually JSON files that satisfy the above format, must be parsed as language files.
{% endhint %}

Once the build succeeds, you can access the `LocalStrings` class, which will be explained in the below page:

{% content-ref url="localization-tools.md" %}
[localization-tools.md](localization-tools.md)
{% endcontent-ref %}

You can use the following additional properties:

*   `AptLocDisableLocalization`: Specifies whether to exclude a localization file from the list of localization files to be parsed. Only effective if you're using wildcards to specify language paths, like below:

    ```xml
    <ItemGroup>
      <AdditionalFiles Include="Resources\Languages\*.json" AptLocIsLanguagePath="true" />
      <AdditionalFiles Include="Resources\Languages\cnt.json" AptLocIsLanguagePath="true" AptLocDisableLocalization="true" />
    </ItemGroup>
    ```
*   `AptLocDisableInvalidCultureWarnings`: Specifies whether to disable invalid culture warnings or not. If you disabled invalid culture warnings, invalid cultures will be considered valid. Otherwise, warnings will be issued for every invalid culture, removing them from the list of language cultures.

    ```xml
    <PropertyGroup>
      <AptLocDisableInvalidCultureWarnings>true</AptLocDisableInvalidCultureWarnings>
    </PropertyGroup>
    ```

{% hint style="info" %}
Culture warnings are issued for invalid cultures according to the host operating system (that is, the host that builds the application).
{% endhint %}
