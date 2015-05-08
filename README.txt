Pseudo field
------------

This module allows you to render an extra field as a real field (with field label).

Description
-----------

The extra fields that can be attached to the entity hook_field_extra_fields() lack
the overall look and feel of real fields because real fields have labels and a theme
around them while extra fields look pretty bare. In order utilize the "field" theme
around extra fields, this module allows you to add "#pseudo_field" attribute to an
extra field render array and it will be themed with "field" theme.

Note: If extra field render array has children arrays, they will be rendered as a multiple
value field.

Example
-------

/**
 * Implements hook_field_extra_fields().
 */
function hook_field_extra_fields() {
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
 */
function hook_node_view($node, $view_mode, $langcode) {
  if ($node->type == 'page') {
    $node->content['single_value'] = array(
      '#pseudo_field' => TRUE,
      '#title' => 'Cat meowing',
      '#markup' => t('Meow!'),
    );
    $node->content['multiple_values'] = array(
      '#pseudo_field' => TRUE,
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
