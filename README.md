# GraphQL Schema

GraphQL schema library.

[![Build Status](https://travis-ci.org/oligus/schema.svg?branch=master)](https://travis-ci.org/oligus/schema)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Codecov.io](https://codecov.io/gh/oligus/schema/branch/master/graphs/badge.svg)](https://codecov.io/gh/oligus/schema)

## Types

*Available types:*

```php
GQLSchema\Types\TypeBoolean
GQLSchema\Types\TypeFloat
GQLSchema\Types\TypeID
GQLSchema\Types\TypeInteger
GQLSchema\Types\TypeObject
GQLSchema\Types\TypeString
```

*Example:*

```php
$type = new TypeBoolean();
$type->__toString(); // Bool

echo $type = new TypeObject(null, 'MyObject');
echo $type->__toString(); // MyObject
```

#### Type modifiers

Type modifiers are used in conjunction with types, add modifier to a type to modify the type in question.

Type                            | Syntax      | Example
--------------------------------| ----------- | -------
Nullable Type                   | \<type>     | String
Non-null Type                   | \<type>!    | String!
List Type                       | [\<type>]   | [String]
List of Non-null Types          | [\<type>!]  | [String!]
Non-null List Type              | [\<type>]!  | [String]!
Non-null List of Non-null Types | [\<type>!]! | [String!]!

*Example:*
```php
$typeModifier = new TypeModifier($nullable = false, $listable = true, $nullableList = false);
$type = new TypeBoolean($typeModifier);

echo $type->__toString(); // [bool!]!
```

## Fields

*Example:*

```php
// Simple
$field = new Field(new TypeInteger(), new ArgumentCollection(), 'simpleField');
$field->__toString(); // simpleField: Int

// With arguments        
$arguments = new ArgumentCollection();
$arguments->add(new Argument(new TypeBoolean(new TypeModifier($nullable = false)), null, 'booleanArg'));
$arguments->add(new Argument(new TypeInteger(new TypeModifier($nullable = false)), null, 'integerArg'));
$arguments->add(new Argument(new TypeString(new TypeModifier($nullable = false)), new ValueString('test'), 'stringArg'));

$field = new Field(new TypeInteger(new TypeModifier(false)), $arguments, 'testField');
echo $field->__toString(); // testField(booleanArg: Boolean!, integerArg: Int!, stringArg: String! = "test"): Int!'

```

## Arguments

Create arguments

*Example:*

```php
// Boolean with no default value
$arg = new Argument(new TypeBoolean(), null, 'argName');
echo $arg->__toString(); // argName: Boolean

// Boolean collection non nullable
$arg = new Argument(new TypeBoolean(new TypeModifier($nullable = true, $listable = true, $nullableList = false), null, 'argName');
echo $arg->__toString(); // argName: [Boolean]!

// Boolean with default value
$arg = new Argument(new TypeBoolean(), new ValueBoolean(false), 'argName');
echo $arg->__toString(); // argName: Boolean = false

```

## Values

Set simple scalar values for default values in the schema. 

*Available values:*

```php
GQLSchema\Values\ValueBoolean
GQLSchema\Values\ValueFloat
GQLSchema\Values\ValueInteger
GQLSchema\Values\ValueNull
GQLSchema\Values\ValueString
```

*Example:*

```php
$bool = new ValueBoolean(true);
$bool->getValue(); // true
echo $bool->__asString(); // 'true'

$float = new ValueFloat(23.45);
$float->getValue(); // 23.45
echo $float->__asString(); // '23.45'

$int = new ValueInteger(5);
$float->getValue(); // 5
echo $float->__asString(); // '5'

$null = new ValueNull();
$null->getValue(); // null
echo $null->__asString(); // 'null'

$string = new ValueString('test string);
$string->getValue(); // 'test string'
echo $string->__asString(); // '"test string"'
```
