# Modify Field Data
## Configuration
To modify field data before being saved into an object a `modification` can be applied to a field configuration.
`modification` should reference a method in the Mapper class or an extension applied to the Mapper class.
```yaml
Dynamic\Salsify\Model\Mapper.example:
  extensions:
    - ExampleModificationExtension
  mapping:
    \Page:
      FrontImageID:
        salsifyField: Front Image
        type: IMAGE
      Title:
          salsifyField: GTIN Name
          modification: exampleModification
```

## Modification Method
The modification function is passed the database field, filed configuration, and the object data.
The data will only be modified for the field it is applied to and will not carry over to other fields.
```php
<?php

use SilverStripe\Core\Extension;
/**
 * Class TestModification
 */
class ExampleModificationExtension extends Extension
{
    /**
     * @param string $dbField
     * @param array $config
     * @param array $data
     * @return array
     */
    public function exampleModification($dbField, $config, $data) {
        if ($dbField === 'Title') {
            // the salsify field in the config for te field
            $salsifyField = $config['salsifyField'];
            // updates the salsify field data
            $data[$salsifyField] = $data[$salsifyField] . ' EXAMPLE_MOD';
        }
        // always return the data
        return $data;
    }
}
```

All title fields that use this mapping for `\Page` will append ` EXAMPLE_MOD` to the end.