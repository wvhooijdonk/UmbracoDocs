---
versionFrom: 9.0.0
versionTo: 10.0.0
meta.Title: Language Files & Localization
description: >-
  Language files are used to translate the Umbraco backoffice user interface so
  that end users can use Umbraco in their native language.
---

# Language Files & Localization

Language files are used for translating:
- the Umbraco backoffice user interface so that end users can use Umbraco in their native language.
  This is particularly important for content editors who do not speak English.
- the password policy errors used by the member login, registration, reset passsword and forgot password components.

If you are a package developer, [see here for docs on how to include translations for your own package](packages/language-files-for-packages.md).

## Supported Languages

Current languages that are included in the core are:

* English (UK)
* English (US)
* Danish
* German
* Spanish
* French
* Hebrew (Israel)
* Italian
* Japanese
* Korean
* Dutch
* Norwegian
* Polish
* Portuguese
* Russian
* Swedish
* Chinese
* Chinese (Taiwan)
* Czech
* Turkish
* Welsh

## Where to find the language files

### Core language files

The core Umbraco language files are found at the following location within the Umbraco source:

```xml
Umbraco-CMS/src/Umbraco.Core/EmbeddedResources/Lang/
```

These language files are the ones shipped with Umbraco and should not be modified.

### Package language files

If you are a package developer, [see here for docs on how to include translations for your own package](packages/language-files-for-packages.md), package language files are located in:

```xml
/App_Plugins/mypackage/Lang/{language}.xml
```

{% hint style="info" %}
The `App_Plugins` version of the `Lang` directory is case sensitive on Linux systems, so make sure that it start with a capital `L`.
{% endhint %}

### User language files

If you want to override Umbraco core translations or translations shipped with packages, you can do that too, these files are located here:

```xml
/config/lang/{language}.user.xml
```

By default, these files are empty but you can add any new keys you want or override existing ones with your own translations. The nice part about the user files is that they will not get overwritten by the installer when you upgrade your Umbraco versions.

In order for these files to deploy when you do a `dotnet publish`, you need to add the following to your `.csproj` file:

```xml
<ItemGroup>
    <Content Include="config/**" CopyToOutputDirectory="Always" />
</ItemGroup>
```

## Using the language keys

Using core or custom language keys from your code:

### From .NET

`ILocalizedTextService` is used to localize strings, and is available through dependency injection. First, inject the service, and then use the `Localize()` method available in the namespace `Umbraco.Extensions` to localize the string with the format \[area]/\[key]:

```csharp
public MyClass(ILocalizedTextService textservice)
{
    var localizedLabel = textservice.Localize("dialog/mykey");
}
```

### From Angular

In the Umbraco backoffice UI, labels can be localized with the `localize` directive:

```xml
<button>
    <localize key="dialog_myKey">Default value</localize>
</button>
```

The localize directive can also be used as an attribute like below where the value of the title attribute is then populated with the dictionary key "title\_name" from the language file using "@title\_name".

```xml
<button localize="title" title="@title_name">
    <localize key="dialog_myKey">Default value</localize>
</button>
```

Or from a controller by using the `LocalizationService` which returns an async translation in a promise:

```javascript
localizationService.localize("dialog_myKey").then(function(value){
                element.html(value);
});
```

## Help keep the language files up to date

As Umbraco is a continually evolving product it is inevitable that new text is added on a fairly regular basis to the English language version of these files. This may mean that some of the above languages are no longer up to date.

If a translation is missing, the key "alias" used will be shown within the user interface, as an example:

```xml
[assignDomain]
```

The language files are XML files with a straight-forward layout as seen below.

```xml
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<language alias="en" intName="English (UK)" localName="English (UK)" lcid="" culture="en-GB">
    <creator>
        <name>The Umbraco community</name>
        <link>https://our.umbraco.com</link>
    </creator>
    <area alias="actions">
        <key alias="assignDomain">Culture and Hostnames</key>
        <key alias="auditTrail">Audit Trail</key>
        ...
    </area>
    ...
</language>
```

In the above example of a missing translation for "assignDomain", locate this string in the en.xml file and then copy the whole "Key" element into the relevant language file. Then you can translate the text, as an example here is the Spanish version of the above snippet:

```xml
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<language alias="es" intName="Spanish" localName="español" lcid="10" culture="es-ES">
    <creator>
        <name>The Umbraco community</name>
        <link>https://our.umbraco.com</link>
    </creator>
    <area alias="actions">
        <key alias="assignDomain">Administrar hostnames</key>
        <key alias="auditTrail">Auditoría</key>
        ...
    </area>
    ...
</language>
```

If you do update any of the core language files or you add a new language, don't forget to help the rest of the community by [submitting a pull request](https://docs.umbraco.com/welcome/contribute/getting-started) so that your changes are merged into the core.
