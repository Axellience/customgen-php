# customgen-php (custom code generation for PHP based on Acceleo - Used in GenMyModel)

customgen-php is a UML to PHP generator written in Acceleo.
This generator provides a way to generate your php classes from a UML class diagram.

For this first version, only the inheritance is managed.




## Translation

Here is the mapping which is applied to UML in order to produce the PHP code.

| UML | PHP |
| :-- | :-- |
|Class (abstract or not) | PHP class |
|Package|										folder (if the class is contents in the package, the PHP file will be created in the folder with the package name.)|
|Attribute|									Attribute of PHP class|
|Generalisation| 								Inheritance between two classes|
|Model| main.php|

## GenMyModel Integration

You can directly integrate this generator into GenMyModel using custom generators.

Clic on `Tools` then `Custom generator`. Add a custom gen and enter the following keys:

    Generator name: PHPGEN (or whatever you want actually)
    Github URL:     https://github.com/Axellience/customgen-php.git
    Github branch:  master (not required)
   
As this generator is public on Github, there is no need to enter login information.
Do not forget to save it and that's all.
You are good to go! You can launch the generator and download the code source (`zip` format).


## Technical Overview

This generator looks over the UML diagram and creates one php file per class.
If the class is contained in a package, the php file will be created in the folder with the package name.
Once the generator is in a class, if the class inherits from another, "extends" and "require" links will be generated.
In the after step, the generator iterates on attributes and their visibility and add the code to a class.
If the attribute visibility is set to "package", then it will be considered as protected.

Here is an example of generated code (from this UML model http://repository.genmymodel.com/ali.gourch/phpExample)

The hierarchy of generated files

    |-- folder_1
    |	|-- folder_2
    |	|	|-- B.php
    |	|-- A.php
    |-- C.php
    |-- D.php
    |-- E.php

B.php
```php
<?php
class B {
	protected $attribute_2;
	public $attribute_3;
	
}
?>
```

D.php
```php
<?php
require_once 'folder_1/folder_2/B.php';

class D extends B {
	protected $attribute_4;
	
}
?>
```

E.php
```php
<?php
require_once 'C.php';

class E extends C {
	public $attribute_5;
	
}
?>
```

Main.php
```php
<?php
require_once 'D.php';

$D = new D();
$D->attribute_3 = "valueOfAttribute_3 \n";

echo $D->attribute_3;

?>
```
