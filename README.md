Pseudo Field
============

This module allows you to render an extra field like a real field with an
optional label.

The extra fields that can be attached to an entity using
`hook_field_extra_fields()` lack the overall look and feel of real fields
because real fields have labels and a theme around them while extra fields
initially look pretty bare (unless you theme them manually).

Just add the `'#pseudo_field'` property set to TRUE to the extra field output
render array and it will render like a real field.

If the extra field render array has children arrays, the extra field will be
rendered as a multiple value field with each child array being an individual
field item.


Installation
------------

Install this module using [the official Backdrop CMS instructions](https://backdropcms.org/guide/modules).

Usage
-------------

In your implementation where you return a render array for the extra fields,
include the following properties in the render array:

- `'#pseudo_field'` — if this property is set to TRUE, the extra field render
array will be rendered with the `field` theme.
- `'#title'` — field label for the extra field. Defaults to the label of the
extra field defined in `hook_field_extra_fields()`.
- `'#label_display'` — defines the position of the extra field label. Available
options are: `'above'`, `'inline'` and `'hidden'`. Defaults to `'above'` or
`'hidden'` if the value of `'#title'` property is empty. (Unfortunately, the
field label position cannot be set for extra fields in the entity's view mode
settings, so this option enables you to set the label position as needed.)
- `'#field_name'` — defines a field name which is used in the
`'field-name-FIELDNAME'` class of the field HTML element. Defaults to the extra
field key defined in `hook_field_extra_fields()`.
- `'#field_type'` - defines the type of the field, which is used in the
`'field-type-FIELDTYPE'` class of the field HTML element. Defaults to
`'extra_field'`.

Example
-------

This example shows how you can add two extra fields to a `page` node.

```
/**
 * Implements hook_field_extra_fields().
 *
 * Adds two extra fields to view modes of "page" content type.
 */
function MODULE_field_extra_fields() {
  $data['node']['page']['display']['single_value'] = array(
    'label' => t('Single value extra field'),
    'weight' => 0,
  );
  $data['node']['page']['display']['multiple_values'] = array(
    'label' => t('Multiple value extra field'),
    'weight' => 1,
  );
  return $data;
}

/**
 * Implements hook_node_view().
 *
 * Provides content for "single_value" and "multiple_values" extra fields.
 */
function MODULE_node_view($node, $view_mode, $langcode) {
  if ($node->type == 'page') {
    // Make extra field look like a single value field. This render array
    // has only markup with no children arrays.
    $node->content['single_value'] = array(
      '#pseudo_field' => TRUE,
      '#title' => 'Cat meowing',
      '#markup' => t('Meow!'),
    );
    // Make extra field look like a multi value field. This render array
    // has children arrays, so each children array will act as a single
    // field value in a multi value field. Label position is set to "inline".
    $node->content['multiple_values'] = array(
      '#pseudo_field' => TRUE,
      '#label_display' => 'inline',
      '#title' => 'Dogs barking',
      0 => array(
        '#markup' => 'Woof woof',
      ),
      1 => array(
        '#markup' => 'Ruff ruff',
      ),
      2 => array(
        '#markup' => 'Yap yap',
      ),
    );
  }
}
```

Issues
------

Bugs and feature requests should be reported in [the Issue Queue](https://github.com/backdrop-contrib/pseudo_field/issues).

Current Maintainers
-------------------

- [Robert J. Lang](https://github.com/bugfolder)

Credits
-------

- Ported to Backdrop CMS by [Robert J. Lang](https://github.com/bugfolder).
- Originally written for Drupal by [Maijs](https://www.drupal.org/u/maijs).

License
-------

This project is GPL v2 software.
See the LICENSE.txt file in this directory for complete text.

