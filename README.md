# siren-enhanced-actions

The following is a series of extensions to the [siren](https://github.com/kevinswiber/siren)
hypermedia specification. The goal is to use this specification as a way to test out new
features in practice, then contributing them upstream for inclusion. As such, these extensions
will aim to be broadly-applicable. (mostly adopting language from HTML)

**NOTE:** These extensions are fully backwards-compatible, so clients that do not support
them should still be able to function normally.


## Actions

The following extensions apply to resource [actions](https://github.com/kevinswiber/siren#actions-1).
Each section below corresponds to a property on an `action` object. Where a property is already
defined by the spec, it is intended to add semantics that were not specified previously. Otherwise,
they should be considered new properties.

### rel

Traditionally, the `name` property of an action is used as a unique identifier. The `rel` property
behaves just like in a `link`. It can either be a
[value defined by the IANA](http://www.iana.org/assignments/link-relations/link-relations.xhtml),
or an absolute URL pointing to a resource that describes the action.

The proposal for siren upstream would be to replace `name` with `rel` and make the discoverability
afforded by it a first-class feature consistent between actions and links. (in the interim, `name`
will remain required for backwards-compatibility)


## Fields

The following extensions apply to action [fields](https://github.com/kevinswiber/siren#fields-1).
Each section below corresponds to a property on an `field` object. Where a property is already
defined by the spec, it is intended to add semantics that were not specified previously. Otherwise,
they should be considered new properties.

### type

When a field uses the type `select`, it can be treated like an HTML `<select>` box. The following
other properties are related:

 - `options`: used to populate the select box with the given options
 - `multiple`: used to indicate that the select supports multiple values
 - `value`: determines which value in the `options` list is selected

### options

Consists of a list of predetermined values that a `select` input can use. It must always be an
array. Each item may be an object that contains a required `value` property and an optional
`title` property.

The `value` represents what will be serialized if that item is selected by the client. The `title`
is a more human-friendly label that can be presented to the user in place of the value. When
`title` is excluded, it is assumed to be identical to `value`.

```json
{
  "name": "color",
  "type": "select",
  "options": [
    { "value": "#ff0000", "title": "Red" },
    { "value": "#00ff00", "title": "Green" },
    { "value": "#0000ff", "title": "Blue" }
  ]
}
```

An item can also be a simple string, in the case where the `value` and `title` are identical, as
a more compact syntax.

```json
{
  "name": "color",
  "type": "select",
  "options": [ "red", "green", "blue" ]
}
```

### multiple

When used in conjunction with a `select` field, this is a boolean that indicates that multiple
values may be chosen by the client. (this corresponds to a `<select>` element's
[`multiple` attribute](https://html.spec.whatwg.org/multipage/forms.html#attr-input-multiple))

### value

When used in conjunction with a `select` field that has `multiple` flag turned on, this could
also become an array of values, where each item indicates that an `option` with the same `value`
must be selected.
