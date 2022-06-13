# VEHICLE SERVICE CATALOG SYNTAX

(C) 2021 - Magnus Feuer

This document is licensed under Attribution 4.0 International License
described [here](https://creativecommons.org/licenses/by/4.0/)

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [VEHICLE SERVICE CATALOG SYNTAX](#vehicle-service-catalog-syntax)
- [FEATURES](#features)
    - [Features currently not included](#features-currently-not-included)
- [NAMESPACE VERSIONING](#namespace-versioning)
- [NATIVE DATA TYPES](#native-data-types)
- [ARRAYS](#arrays)
- [VALUE RANGE SPECIFICATION](#value-range-specification)
    - [Using regular expressions](#using-regular-expressions)
    - [Using sets](#using-sets)
    - [Using intervals](#using-intervals)
    - [Range specification for native types](#range-specification-for-native-types)
    - [Range specification for enumerations](#range-specification-for-enumerations)
    - [Range specification for complex types](#range-specification-for-complex-types)
    - [Range specifications for arrays](#range-specifications-for-arrays)
- [RELATIVE VS. ABSOLUTE DEFINED DATA TYPE REFERENCE](#relative-vs-absolute-defined-data-type-reference)
    - [Local namespace data type](#local-namespace-data-type)
    - [Nested namespace data type](#nested-namespace-data-type)
    - [External namespace data type](#external-namespace-data-type)
- [DEPLOYMENT FILES](#deployment-files)
    - [Deployment file overrides](#deployment-file-overrides)
    - [Deployment file object list extensions](#deployment-file-object-list-extensions)
- [VSC FILE SYNTAX, SEMANTICS AND STRUCTURE](#vsc-file-syntax-semantics-and-structure)
    - [Key-Object: `namespaces`](#key-object-namespaces)
        - [Namespace key-value: `name`](#namespace-key-value-name)
        - [Namespace key-value: `description`](#namespace-key-value-description)
        - [Namespace key-value: `major-version`](#namespace-key-value-major-version)
        - [Namespace key-value: `minor-version`](#namespace-key-value-minor-version)
    - [Namespace list object: `typedefs`](#namespace-list-object-typedefs)
        - [Typedef key-value: `name`](#typedef-key-value-name)
        - [Typedef key-value: `description`](#typedef-key-value-description)
        - [Typedef key-value: `type`](#typedef-key-value-type)
        - [Typedef key-value: `datatype`](#typedef-key-value-datatype)
        - [Typedef key-value: `arraysize`](#typedef-key-value-arraysize)
        - [Typedef key-value: `min`](#typedef-key-value-min)
        - [Typedef key-value: `max`](#typedef-key-value-max)
    - [Namespace list object: `structs`](#namespace-list-object-structs)
        - [Struct key-value: `name`](#struct-key-value-name)
        - [Struct key-value: `description`](#struct-key-value-description)
        - [Struct list object: `members`](#struct-list-object-members)
            - [Member key-value: `name`](#member-key-value-name)
            - [Member key-value: `description`](#member-key-value-description)
            - [Member key-value: `datatype`](#member-key-value-datatype)
            - [Member key-value: `arraysize`](#member-key-value-arraysize)
    - [Namespace list object: `enumerations`](#namespace-list-object-enumerations)
        - [Enumeration key-value: `name`](#enumeration-key-value-name)
        - [Enumeration key-value: `datatype`](#enumeration-key-value-datatype)
        - [Enumeration key-value: `description`](#enumeration-key-value-description)
        - [Enumeration list object: `options`](#enumeration-list-object-options)
            - [Options key-value: `name`](#options-key-value-name)
            - [Options key-value: `value`](#options-key-value-value)
            - [Options key-value: `description`](#options-key-value-description)
    - [Namespace list Object: `methods`](#namespace-list-object-methods)
        - [Methods key-value: `name`](#methods-key-value-name)
        - [Methods key-value: `description`](#methods-key-value-description)
        - [Methods list object: `in`](#methods-list-object-in)
            - [In parameter key-value: `name`](#in-parameter-key-value-name)
            - [In parameter key-value: `description`](#in-parameter-key-value-description)
            - [In parameter key-value: `datatype`](#in-parameter-key-value-datatype)
            - [In parameter key-value: `arraysize`](#in-parameter-key-value-arraysize)
            - [In parameter key-value: `range`](#in-parameter-key-value-range)
        - [Methods list object: `out`](#methods-list-object-out)
            - [Out parameter key-value: `name`](#out-parameter-key-value-name)
            - [Out parameter key-value: `description`](#out-parameter-key-value-description)
            - [Out parameter key-value: `datatype`](#out-parameter-key-value-datatype)
            - [Out parameter key-value: `arraysize`](#out-parameter-key-value-arraysize)
            - [Out parameter key-value: `range`](#out-parameter-key-value-range)
        - [Methods list object: `error`](#methods-list-object-error)
            - [Error parameter key-value: `datatype`](#error-parameter-key-value-datatype)
            - [Error parameter key-value: `arraysize`](#error-parameter-key-value-arraysize)
            - [Error parameter key-value: `range`](#error-parameter-key-value-range)
    - [Namespace list object: `events`](#namespace-list-object-events)
        - [Event key-value: `name`](#event-key-value-name)
        - [Event key-value: `description`](#event-key-value-description)
        - [Event list object: `in`](#event-list-object-in)
            - [In parameter key-value: `name`](#in-parameter-key-value-name-1)
            - [In parameter key-value: `description`](#in-parameter-key-value-description-1)
            - [In parameter key-value: `datatype`](#in-parameter-key-value-datatype-1)
            - [In parameter key-value: `arraysize`](#in-parameter-key-value-arraysize-1)
            - [In parameter key-value: `range`](#in-parameter-key-value-range-1)
    - [Namespace list object: `properties`](#namespace-list-object-properties)
        - [Property key-value: `name`](#property-key-value-name)
        - [Property key-value: `description`](#property-key-value-description)
        - [Property key-value: `datatype`](#property-key-value-datatype)
        - [Property key-value: `arraysize`](#property-key-value-arraysize)
        - [Property parameter key-value: `range`](#property-parameter-key-value-range)
    - [Namespace list object: `includes`](#namespace-list-object-includes)
        - [Include key-value: `file`](#include-key-value-file)
        - [Include key-value: `description`](#include-key-value-description)

<!-- markdown-toc end -->


This document describes the structure of the Vehicle Service Catalog
(VSC) YAML file.

------------------

# FEATURES
The format supports the following features

* **Namespaces**  
  Logical grouping of methods, events, properties, and defined data types that can be nested.

* **Methods**  
  A call, executed by a single server instance, that optionally returns a value.
  Execution is guaranteed to TCP level with server failure being reported.
  
* **Events**  
  A fire-and-forget call, executed by zero or more subscribing instances, that does not return a value.
  Execution is best effort to UDP level with server failures not being reported.

* **Defined data types**  
  Named data types that can be enumerations, (nested) structs or typedefs.
  
* **Properties**  
  A shared state object that can be read and set, and which is
  available to all subscribing entities. Compared with a signal (see
  below), a property can be viewed as a level trigger while a signal
  is an edge trigger.

* **Deployment files**  
  Adds deployment-specific data to a VSC file.
  
## Features currently not included

The following features are yet to be determined:

* **Signals**  
  These are semantically equivalent to single-argument events. We need to decide
  how we want to integrate VSS signals.
  
* **More?**


--------------------

# NAMESPACE VERSIONING

VSC namespaces can optionally have a major and minor version,
specified by `major-version` and `minor-version` keys.

Namespace version management lets a client implementation have
expectations that a server implementation will support a specific
variant of data types, method call, or property.

Bumped minor numbers identifies backward-compatible additions to the
previous version.  This means that if a client requires version 1.3 of
a server namespace and its methods, it knows that it can safely
interface a server implementation of version 1.4 of that same
namespace.

Bumped major versions identifies non-compatible changes to the
previous version, such as a changed method signature. This means that
a client requiring version 1.3 of a server knows that it cannot invoke
a version 2.0 server implementation of that interface since that
implementation is not backward compatible.

Namespace versioning can be used build-time to ensure that the correct
version of all needed namespace implementations are deployed, while
also detecting if multiple, non-compatible versions of a namespace is
required.

-------------------

# NATIVE DATA TYPES

The following native data types are available as properties, in/out
arguments, enumeration types, and struct members.

(Imported from VSS)

| Name       | Type                                   | Min         | Max        |
|:-----------|:---------------------------------------|:------------|:-----------|
| uint8      | unsigned 8-bit integer                 | 0           | 255        |
| int8       | signed 8-bit integer                   | -128        | 127        |
| uint16     | unsigned 16-bit integer                | 0           | 65535      |
| int16      | signed 16-bit integer                  | -32768      | 32767      |
| uint32     | unsigned 32-bit integer                | 0           | 4294967295 |
| int32      | signed 32-bit integer                  | -2147483648 | 2147483647 |
| uint64     | unsigned 64-bit integer                | 0           | 2^64 - 1   |
| int64      | signed 64-bit integer                  | -2^63       | 2^63 - 1   |
| boolean    | boolean value                          | 0/false     | 1/true     |
| float      | floating point number                  | -3.4e -38   | 3.4e 38    |
| double     | double precision floating point number | -1.7e -300  | 1.7e 300   |
| string     | character string                       | n/a         | n/a        |
| byteBuffer | buffer of bytes (aka BLOB)             | n/a         | n/a        |

-------------

# ARRAYS

Besides the datatypes described above, VSS supports as well the concept of
`arrays`, as a *collection of elements based on the data entry
definition*, wherein it's specified. By default the size of the array is undefined.
By the optional keyword `arraysize` the size of the array can be specified.
The following syntax shall be used to declare an array:

```YAML
# Array of datatype uint32, by default size of the array is undefined
datatype: uint32[]

# Optional: specified number of elements in the array
arraysize: 5
```

Only single-dimensional arrays are supported.

---------------
# VALUE RANGE SPECIFICATION

Methods, events, and properties can optionally specify a range of
legal value that their `in`, `out`, `error` and property can take.

A range is specified as a list of values that are legal for a given
parameter or property to have.

Range definitions can be used to verify that the contract of a method,
event, or property is not violated by transmitting illegal values
between caller and callee, or publisher and subscriber in the case of
properties.

If `range` is omitted, any value that can be carried by the given
datatype is a legal value.

A range is specified as a string with a boolean expression consisting
of relational operators and operands set to the value being verified.

The `$` operand is the value being verified.

String values are double quoted (`"`). Double quotes within a string are expressed as two double quotes (`""`).


`$ == 10 or $ == 20`

`$ in_set (1,3,4,5)`

`$ <= 20`

`$ == "a value"`

`$ == "a ""double-quoted"" value"`

`$ != 1.2 and ($ < 1.0 or $ > 1.4)`

If the expression evaluates to true, the value is considered legal

## Using regular expressions

A range expression can use regular expressions to match against value patterns.

`regex("^the_.+_value$") or regex("^a_.+value$") or $ == "something_else"`

Examples of legal values in this case would be `the_first_value`,
`the_second_value`, `a_1234_value`, `something_else`.


## Using sets

A range expression can have a set:

`$ in_set (1,2,5,10)`

In this case `1`, `2`, `5`, and `10` are legal values while any other
values are illegal.

A set can be strings:

`$ in_set ("first", "second", "fourth")`

**Note** A set cannot include a `regex()` expression.

A set can be floats:

`$ in_set (999.99, 11.2, 848222.442)`

## Using intervals

A range expression can have an interval:

`$ in_interval (20,23)`

In this case the legal values for the value are `20`, `21`, `22`, and `23`.

For strings, lexigraphic intervals are used:

`$ in_interval ("aaa", "aad")`

In this case the legal values for the value are `aaa`, `aab`, `aac`, and `aad`.

An inteval can be floats:

`$ in_interval (-1.0, 1.0)`

## Range specification for native types

In its simplest form, a value range is specified as simple native data
type comparisons relations, as shown below.

```YAML
methods:
  - name: my_method
    in:
      - name: an_int_argument
        datatype: int8
        range: $ < 100 and $ > 10

      - name: a_string_argument
        datatype: string
        range: $ == "hello" or $ == "world"
```


If the range expression, with `$` substituted by the value to verify, evaluates to true, then the value is legal.


## Range specification for enumerations

Parameters, errors, and properties that have an enum value specify
their range using the the symbolic values, as shown below:


```YAML
enumerations:
  - name: error_t
    type: enumeration

    datatype: int16
    options:
      - name: "null"
        value: 0

      - name: ok
        value: 1

      - name: in_progress
        value: 2

methods:
  - name: my_method
    error:
      datatype: .stdvsc.error_t
      # Symbolic enum value
      range: $ == "ok" or $ == "in_progress"
```


## Range specification for complex types

Complex (struct) values specify their ranges using member specification after the `$` token, as shown below

```YAML
structs:
  - name: seat_location_t
    members:
      - name: row
        datatype: uint8
      - name: index
        datatype: uint8

methods:
  - name: move
    in:
      - name: location
        datatype: seat_location_t
        range:  $.row >= 0 and $.row <= 3 and $.index in_interval(1, 10)
```

Nested complex types can be specified as well by using nested members:

```YAML
structs:
  - name: inner_struct_t
    members:
      - name: inner_struct_value_1
        datatype: int8

      - name: inner_struct_value_2
        datatype: int32

  - name: outer_struct_t
    members:
      - name: outer_struct_value_1
        datatype: string

      - name: outer_struct_value_2
        datatype: float

      - name: an_inner_struct
        datatype: inner_struct_t

methods:
  - name: my_function
    in:
      - name: my_complex_parameter
        datatype: outer_struct
        range: >
            $.outer_struct_value_1 in_set("first", "second", "fourth", "eigth") and 
            $.outer_struct_value_2 < 2.0 and
            ( $.an_inner_struct.inner_struct_value_1 == 1 or
              $.an_inner_struct.inner_struct_value_2 == 123456789 )
```


## Range specifications for arrays

Range specifications for arrays are defined by adding an index
(starting with `0`) to the `$` token, as shown below:


```YAML
methods:
  - name: my_method
    in:
      - name: an_array_argument
        datatype: int8[]
        arraysize: 3
        range: $[0] == 10 and $[1] == 20 and $[2] == 30
```

A range expression that uses an element index that does not exist in
the tested value will render the expression false. This is especially
important for unbound arrays where the value can host any number of
elements.


If the value is of a complex type where the type's members are arrays,
a combination of member names and array indices can be used, as shown
below:


```YAML
structs:
  - name: struct_with_array_members_t
    members:
      - name: array_member_1
        datatype: uint8[]
        array_size: 10

      - name: array_member_2
        datatype: string[]
        array_size: 5

methods:
  - name: my_method
    in:
      - name: my_argument
        datatype: struct_with_array_members_t
        range:  $.array_member_1[0] >= 0 and $.array_member_2[4] != "a value"
```


---------------


# RELATIVE VS. ABSOLUTE DEFINED DATA TYPE REFERENCE

A defined data type, specified in a `typedefs`, `structs`, or
`enumerations` list object, can be referenced by other entities such
as other data types(such as a struct member being another struct or
enumerator), methods and event parameters, and properties.

These references can be of three different kinds:

* **Local namespace data type**  
The data type lives in the same namespace as the entity using it.

* **Nested namespace data type**  
The data type lives in a namespace nested under the namespace of the
entity using the type.

* **External namespace data type**  
The data type lives in a namespace outside the namespace of the
entity using the type.

## Local namespace data type
In this case the data type is defined in the same namespace as the entity
using it, as shown below


```YAML
namespaces:
  - name: my_namespace
    typedefs:
      - name: my_typedef
        datatype: int16
        
    methods:
      - name: my_method
        in:
          - name: an_argument
            datatype: my_typedef
```

In this case `my_method` has a single argument, `an_argument` that is
of the data type `my_typedef`.

Since both `my_method` and `my_typedef` is defined in the same
namespace (`my_namespace`), the reference to the defined type can
simply be the type name.

##  Nested namespace data type
In this case the data type is defined in a another namespace that resides under
the namespace of the entity using the type, as shown below


```YAML
namespaces:
  - name: my_namespace
    namespaces:
      - name: nested_namespace:
        namespaces:
        - name: second_level_nested_namespace:
          typedefs:
            - name: my_typedef
              datatype: int16
        

    methods:
      - name: my_method
        in:
          - name: an_argument
            datatype: nested_namespace.second_level_nested_namespace.my_typedef
```

In this case `my_method` has a single argument, `an_argument` that is
of the data type `my_typedef`, which is defined two namespaces down.

In other words, `my_namespace` hosts `nested_namespace`, which in its
turn hosts `second_level_nested_namespace`, which in its turn defines
`my_typedef`,

The `my_method` method can reference the defined data type by
prefixing its name with a period-separated namespace path to `my_typedef`:

    datatype: nested_namespace.second_level_nested_namespace.my_typedef

This syntax allows any defined data type of any nested namespace to be referenced.

##  External namespace data type
In this case the data type is defined in a another namespace that
resides under the namespace of the entity using the type, as shown
below:


```YAML
namespaces:
  - name: external_namespace
    namespaces:
      - name: nested_namespace:
        typedefs:
          - name: my_typedef
            datatype: int16

  - name: my_namespace
    methods:
      - name: my_method
        in:
          - name: an_argument
            datatype: .external_namespace.nested_namespace.my_typedef
```

In this case the `my_method` cannot access `my_typedef` through a
nested namespace access since `external_namespace` lives parallel to
`my_namespace` and is not nested inside it.

This is resolved by specifying an absolute path to the defined
datatype, which is identical to a a nested namespace path
apart from being prefixed with a period:

    datatype: .external_namespace.nested_namespace.my_typedef
    
This syntax allows any defined data type anywhere in the tree to be used.

-----------------------

# DEPLOYMENT FILES

Deployment files contains VSC file extensions to be applied when the
VSC file is processed.  An example of deployment file data is a DBUS
interface specification to be used for a namespace, or a SOME/IP
method ID to be used for a method call.

By separating the extension data into their own deployment files the
core VSC specification can be kept independent of deployment details
such as network protocols and topology.

An example of a VSC file sample and a deployment file extension to
that sample is given below:


**File: `comfort-service.yml`**  
```YAML
name: comfort
namespaces:
  - name: seats
    description: Seat interface and datatypes.

    structs: ...
    methods: ...
   ...
```

**File: `comfort-dbus-deployment.yml`**  

```YAML
name: comfort
namespaces: 
  - name: seats
    dbus_interface: com.genivi.cabin.seat.v1
```

The combined YAML structure to be processed will look like this:

```YAML
name: comfort
namespaces: 
  - name: seats
    description: Seat interface and datatypes.
    dbus_interface: com.genivi.cabin.seat.v1

    structs: ...
    methods: ...
```

The semantic difference between a regular VSC file included by an
`includes` list object and a deployment file is that the deployment
file does not have any restrictions on the keys that it adds.  In the
example above, the `dbus_interface` key-value pair can only be added
in a deployment file since `dbus_interface` is not a part of the
regular VSC file syntax.

## Deployment file overrides
If a deployment file key-value element is also defined in the VSC
file, the deployment file's value will be used.

Example:

**File: `comfort-service.yml`**  
```YAML
name: comfort
  typedefs:
    - name: movement_t
      datatype: int16
      min: -1000
      max: 1000
      description: The movement of a seat component
```

**File: `redefine-movement-type.yml`**  
```YAML
name: comfort
  typedefs:
    - name: movement_t
      datatype: int8 # Replaces int16 of the original type
```

The combined YAML structure to be processed will look like this:

```YAML
name: comfort
  typedefs:
    - name: movement_t
      datatype: int8 # Replaced datatype
      min: -1000
      max: 1000
      description: The movement of a seat component
```

## Deployment file object list extensions

If a deployment file's object list element (e.g. `events`) is also
defined in the VSC file, the VSC's list will traversed recursively and
extended by the deployment file's corresponding list.

**FIXME** Possibly add description on how various edge cases are resolved.

Example:

**File: `comfort-service.yml`**  
```YAML
name: comfort
events:
  - name: seat_moving
    description:  The event of a seat starting or stopping movement
    in:
      - name: status
        datatype: uint8
      - name: row
        datatype: uint8

```

**File: `add_seat_moving_in_parameter.yml`**  
```YAML
name: comfort
events:
- name: seat_moving: 
    in:
      - name: extended_status_text
        datatype: string
```

The combined YAML structure to be processed will look like this:

```YAML
name: comfort
events:
  - name: seat_moving
    description:  The event of a seat starting or stopping movement
    in:
      - name: status
        datatype: uint8
      - name: row
        datatype: uint8
      - name: extended_status_text
        datatype: string
```

----------

# VSC FILE SYNTAX, SEMANTICS AND STRUCTURE

A Vehicle Service Catalog is stored in one or more YAML files.  The
root of each YAML file is assumed to be a `namespace` object and needs
to contain at least a `name` key, and, optionally, a `description`. In
addition to this other namespaces, `includes`, `datatypes`, `methods`,
`events`, and `properties` can be specified.

A complete VSC file example is given below:

```YAML
---
name: comfort
major-version: 2
minor-version: 1
description: A collection of interfaces pertaining to cabin comfort.

# Include generic error enumeration to reside directly
# under comfort namespace
includes:
  - file: vsc-error.yml
    description: Include standard VSC error codes used by this namespace

namespaces:
  - name: seats
    description: Seat interface and datatypes.

    typedefs:
      - name: movement_t
        datatype: uint16
  
    structs:
      - name: position_t
        description: The position of the entire seat
        members:
          - name: base
            datatype: movement_t
            description: The position of the base 0 front, 1000 back
    
          - name: recline
            datatype: movement_t
            description: The position of the backrest 0 upright, 1000 flat
    
    enumerations:
      - name: seat_component_t
        datatype: uint8
        options:
          - name: base
            value: 0
          - name: recline
            value: 1
    
    methods:
      - name: move
        description: Set the desired seat position
        in: 
          - name: seat
            description: The desired seat position
            datatype: movement.seat_t
    
    
    events:
      - name: seat_moving
        description: The event of a seat beginning movement
        in:
          - name: status
            description: The movement status, moving (1), not moving (0)
            datatype: uint8
    
    properties:
      - name: seat_moving
        description: Specifies if a seat is moving or not
        type: sensor
        datatype: uint8
```


The following chapters specifies all YAML objects and their keys
supported by VSC.  The "Lark grammar" specification refers to the Lark
grammar that can be found [here](https://github.com/lark-parser/lark).
The terminals used in the grammar (`LETTER`, `DIGIT`, etc) are
imported from
[common.lark](https://github.com/lark-parser/lark/blob/master/lark/grammars/common.lark)


-----------------

## Key-Object: `namespaces`

|                        |                                                                                                     |
|:-----------------------|:----------------------------------------------------------------------------------------------------|
| **Hosted by**          | `namespaces` list, [root element]                                                                   |
| **Mandatory keys**     | `name`                                                                                              |
| **Optional keys**      | `description`, `major-version`, `minor-version`                                                     |
| **Opt. hosted lists**  | `namespaces`, `includes`, `typedefs`,  `structs`, `enumerations`, `methods`, `events`, `properties` |
| **Mand. hosted lists** | N/A                                                                                                 |

A namespace is a logical grouping of other objects, allowing for
separation of datatypes, methods, events, and properties into their
own spaces that do not interfere with identically named objects in
other namespaces.

Namespaces can be nested inside other namespaces to an arbitrary
depth, building up a scalable namespace tree.

Namespaces can be reused either locally in a single file via YAML
anchors, or across YAML files using the `includes` object.

The root of a YAML file is assumed to be a `namespaces` object, and
can contain the keys listed below.

A namespace example is given below.

```YAML
namespaces:
  - name: seats
    major-version: 1
    minor-version: 3
    description: Seat interface and datatypes.
```

### Namespace key-value: `name`
|               |        |
|:--------------|:-------|
| **YAML Type** | string |
| **Mandatory** | Yes    |
| **Lark grammar**  | `LETTER ("_"\|LETTER\|DIGIT)*` |

Specifies the name of the namespace.

### Namespace key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

A description of the namespace.

### Namespace key-value: `major-version`
|                  |                |
|:-----------------|:---------------|
| **YAML Type**    | integer        |
| **Mandatory**    | No             |
| **Lark grammar** | [YAML integer] |

Describes the major version of the namespace.

Major versions are bumped when an existing data type, method, event,
or property hosted by the namespace has its signature changed in a non
backward-compatible way.

Examples are changed datatypes for an input parameter, an added struct
member, or changed values in an enumeration option.

### Namespace key-value: `minor-version`
|                  |                |
|:-----------------|:---------------|
| **YAML Type**    | integer        |
| **Mandatory**    | No             |
| **Lark grammar** | [YAML integer] |

Describes the minor version of the namespace.

Major versions are bumped when data types, methods, events, or properties are added without impacting
backwards compatibility.

A client expecting version 1.3 of a namespace can still use a version
1.4 implementation of the namespace since it only has additions and no
changes to the existing objects.

-----------------


## Namespace list object: `typedefs`
|                        |                                          |
|:-----------------------|:-----------------------------------------|
| **Hosted by**          | `namespaces` list                        |
| **Mandatory Keys**     | `name`, `datatype`                       |
| **Optional keys**      | `description`, `min`, `max`, `arraysize` |
| **Opt. hosted lists**  | N/A                                      |
| **Mand. hosted lists** | N/A                                      |

Each `typedefs` list object aliases an existing primitive type,
defined type, or enumerator, giving it an additional name.

The new data type can be used by other datatypes, method & event
parameters, and properties.

A `typedefs` list object example is given below:

```
typedefs:
  - name: movement_t
    datatype: int16
    min: -1000 
    max: 1000
    description: The movement of a seat component
```

### Typedef key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Specifies the name of the typedef. This name can be used as an alias
of the given datatype.


### Typedef key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Contains a description of the defined type.



### Typedef key-value: `type`
|                  |             |
|:-----------------|:------------|
| **YAML Type**    | string      |
| **Mandatory**    | Yes         |
| **Lark grammar** | `"typedef"` |

Specifies that this `datatypes` list object defines a typedef. 


### Typedef key-value: `datatype`
|                  |                                 |
|:-----------------|:--------------------------------|
| **YAML Type**    | string                          |
| **Mandatory**    | Yes                             |
| **Lark grammar** | `"."? CNAME ("." CNAME)* "[]"?` |

Specifies the data type to use as a source for the typedef.

If the data type is an array (ending with `[]`), the `arraysize` key
can optionally be provided to specify the number of elements in the
array.

If `arraysize` is not specified for an array type, the array can
contain an arbitrary number of elements.

###  Typedef key-value: `arraysize`
|                  |                    |
|:-----------------|:-------------------|
| **YAML Type**    | int                |
| **Mandatory**    | No                 |
| **Lark grammar** | [positive integer] |

Specifies the number of elements in the array.

This key is only allowed if the `datatype` element specifies an array
(ending with `[]`).


### Typedef key-value: `min`
|                  |                |
|:-----------------|:---------------|
| **YAML Type**    | int or decimal |
| **Mandatory**    | No             |
| **Lark grammar** | int or decimal |

Specifies the minimum value that the defined type can be set to.

This key can only be specified if `datatype` is set to `int8`,
`uint8`, `int16`, `uint16`, `int32`, `uint32`, `int64`, `uint64`,
`float`, or `double`.

This key can only be specified 
This key is not allowed if the `datatype` specifies an array.


### Typedef key-value: `max`
|                  |                |
|:-----------------|:---------------|
| **YAML Type**    | int or decimal |
| **Mandatory**    | No             |
| **Lark grammar** | int or decimal |

Specifies the maximum value that the defined type can be set to.

This key can only be specified if `datatype` is set to `int8`,
`uint8`, `int16`, `uint16`, `int32`, `uint32`, `int64`, `uint64`,
`float`, or `double`.

This key can only be specified 
This key is not allowed if the `datatype` specifies an array.

------------------------

## Namespace list object: `structs`

|                        |                   |
|:-----------------------|:------------------|
| **Hosted by**          | `namespaces` list |
| **Mandatory Keys**     | `name`            |
| **Optional keys**      | `description`     |
| **Mand. hosted lists** | `members`         |
| **Opt. hosted lists**  | N/A               |


Each `structs` list object specifies an aggregated data type.

The new data type can be used by other datatypes, method & event
parameters, and properties.

A `structs` list object example, using `movement_t` from the previous `typedefs`
example is shown below:

```
structs:
  - name: position_t
    description: The complete position of a seat
    members:
      - name: base
        datatype: movement_t
        description: The position of the base 0 front, 1000 back

      - name: recline
        datatype: movement_t
        description: The position of the backrest 0 upright, 1000 flat

      - name: lumbar
        datatype: movement_t
        description: The position of the lumbar support
```

### Struct key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Defines the name of the struct


### Struct key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Specifies a description of the struct.


### Struct list object: `members`
|                    |                       |
|:-------------------|:----------------------|
| **Hosted by**      | `structs` list object |
| **Mandatory Keys** | `name`, `datatype`    |
| **Optional keys**  | `description`         |

Each `members` list object defines an additional member of the struct.

Each member can be of a native or defined datatype.

Please see the `struct` sample code above for an example of how 
`members` list objects are used.

#### Member key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Specifies the name of the struct member.


#### Member key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Contains a description of the struct member.


#### Member key-value: `datatype`
|                  |                                 |
|:-----------------|:--------------------------------|
| **YAML Type**    | string                          |
| **Mandatory**    | Yes                             |
| **Lark grammar** | `"."? CNAME ("." CNAME)* "[]"?` |

Specifies the data type of the struct member. 

The type can be either a native or defined type.

If `datatype` refers to a defined type, this type can be a
local, nested, or externally defined reference.

If the type is an array (ending with `[]`), the `arraysize` key can
optionally be provided to specify the number of elements in the array.

If `arraysize` is not specified for an array type, the member array
can contain an arbitrary number of elements.

#### Member key-value: `arraysize`
|                  |                    |
|:-----------------|:-------------------|
| **YAML Type**    | int                |
| **Mandatory**    | No                 |
| **Lark grammar** | [positive integer] |

Specifies the number of elements in the struct member array.

This key is only allowed if the `datatype` element specifies an array
(ending with `[]`).

----------------------

## Namespace list object: `enumerations`

|                        |                           |
|:-----------------------|:--------------------------|
| **Hosted by**          | `namespaces` list         |
| **Mandatory Keys**     | `name`                    |
| **Optional keys**      | `description`, `datatype` |
| **Mand. hosted lists** | `options`                 |
| **Opt. hosted lists**  | N/A                       |

Each `enumerations` list object specifies an enumerated list (enum) of
options, where each option can have its own integer value.

The new data type defined by the enum can be used by other
datatypes, method & event parameters, and properties.

A `enumerations` example list object is given below:

```
enumerations:
  - name: seat_component_t
    datatype: uint8 
    options:
      - name: base
        value: 0

      - name: cushion
        value: 1

      - name: lumbar
        value: 2

      - name: side_bolster
        value: 3

      - name: head_restraint
        value: 4
```

### Enumeration key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Defines the name of the enum.

### Enumeration key-value: `datatype`
|                  |                            |
|:-----------------|:---------------------------|
| **YAML Type**    | string                     |
| **Mandatory**    | No                         |
| **Lark grammar** | `"."? CNAME ("." CNAME)* ` |

Specifies the data type that should be used to host this enum. 

The type can be either a native or defined type, but must resolve
to a native integer type.

If `datatype` refers to a defined type, this type can be a
local, nested, or externally defined reference.


### Enumeration key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Specifies a description of the enum.

### Enumeration list object: `options`
|                    |                            |
|:-------------------|:---------------------------|
| **Hosted by**      | `enumerations` list object |
| **Mandatory Keys** | `name`                     |
| **Optional keys**  | `description`,`value`      |

Each `options` list object adds an option to the enumerator.

Please see the `enumerations` sample code above for an example of how 
`options` list objects are used.

#### Options key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Specifies the name of the enum option.

#### Options key-value: `value`
|                  |                |
|:-----------------|:---------------|
| **YAML Type**    | int            |
| **Mandatory**    | Yes            |
| **Lark grammar** | [YAML integer] |

Specifies the value of the enum option.


#### Options key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Contains a description of the enum option.

----------------------

## Namespace list Object: `methods`
|                        |                      |
|:-----------------------|:---------------------|
| **Hosted by**          | `namespaces` list    |
| **Mandatory Keys**     | `name`               |
| **Optional keys**      | `description`        |
| **Mand. hosted lists** | N/A                  |
| **Opt. hosted lists**  | `in`, `out`, `error` |

Each `methods` list object specifies a method call, executed by a
single server instance, that optionally returns a value. 
Execution is guaranteed to TCP level with server failure being reported.

A `methods` sample list object is given below:

```
methods:
  - name: current_position
    description: Get the current position of a seat

    in:
      - name: row
        description: The desired seat to row query
        datatype: uint8
        range: $ < 10 and $ > 2

      - name: index
        description: The desired seat index to query
        datatype: uint8
        range: $ in_interval(1,4)

    out:
      - name: seat
        description: The state of the requested seat
        datatype: seat_t

    error:
      datatype: .stdvsc.error_t
      range: $ in_set("ok", "in_progress", "permission_denied")

```

### Methods key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Defines the name of the method.

### Methods key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Specifies a description of the method.

### Methods list object: `in`
|                    |                                |
|:-------------------|:-------------------------------|
| **Hosted by**      | `methods` list object          |
| **Mandatory Keys** | `name`, `datatype`             |
| **Optional keys**  | `description`,`value`, `range` |

Each `in` list object defines an input parameter to the method

Please see the `methods` sample code above for an example of how 
`in` parameter lists are used


#### In parameter key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Specifies the name of the input parameter


#### In parameter key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Contains a description of the input parameter.

#### In parameter key-value: `datatype`
|                  |                                 |
|:-----------------|:--------------------------------|
| **YAML Type**    | string                          |
| **Mandatory**    | Yes                             |
| **Lark grammar** | `"."? CNAME ("." CNAME)* "[]"?` |

Specifies the data type of the input argument,

The type can be either a native or defined type.

If `datatype` refers to a defined type, this type can be a
local, nested, or externally defined reference.

If the type is an array (ending with `[]`), the `arraysize` key can
optionally be provided to specify the number of elements in the array.

If `arraysize` is not specified for an array type, the member array
can contain an arbitrary number of elements.

#### In parameter key-value: `arraysize`
|                  |                    |
|:-----------------|:-------------------|
| **YAML Type**    | int                |
| **Mandatory**    | No                 |
| **Lark grammar** | [positive integer] |

Specifies the number of elements in the input parameter array.

This key is only allowed if the `datatype` element specifies an array
(ending with `[]`).

#### In parameter key-value: `range`
|                  |                             |
|:-----------------|:----------------------------|
| **YAML Type**    | string                      |
| **Mandatory**    | No                          |
| **Lark grammar** | [See separate grammar file] |


Specifies the legal range for the value.

Please see [value range specification](#value-range-specification)
chapter for details on how to specify ranges.


### Methods list object: `out`
|                    |                               |
|:-------------------|:------------------------------|
| **Hosted by**      | `methods` list object         |
| **Mandatory Keys** | `name`, `datatype`            |
| **Optional keys**  | `description`,`value`,`range` |

Each `out` list object defines an output parameter to the method

Please see the `methods` sample code above for an example of how 
`out` parameter lists are used

#### Out parameter key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Specifies the name of the output parameter

#### Out parameter key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Contains a description of the output parameter.

#### Out parameter key-value: `datatype`
|                  |                                 |
|:-----------------|:--------------------------------|
| **YAML Type**    | string                          |
| **Mandatory**    | Yes                             |
| **Lark grammar** | `"."? CNAME ("." CNAME)* "[]"?` |

Specifies the data type of the output argument,

The type can be either a native or defined type.

If `datatype` refers to a defined type, this type can be a
local, nested, or externally defined reference.

If the type is an array (ending with `[]`), the `arraysize` key can
optionally be provided to specify the number of elements in the array.

If `arraysize` is not specified for an array type, the member array
can contain an arbitrary number of elements.

#### Out parameter key-value: `arraysize`
|                  |                    |
|:-----------------|:-------------------|
| **YAML Type**    | int                |
| **Mandatory**    | No                 |
| **Lark grammar** | [positive integer] |

Specifies the number of elements in the output parameter array.

This key is only allowed if the `datatype` element specifies an array
(ending with `[]`).


#### Out parameter key-value: `range`
|                  |                             |
|:-----------------|:----------------------------|
| **YAML Type**    | string                      |
| **Mandatory**    | No                          |
| **Lark grammar** | [See separate grammar file] |


Specifies the legal range for the value.

Please see [value range specification](#value-range-specification)
chapter for details on how to specify ranges.


### Methods list object: `error`
|                    |                               |
|:-------------------|:------------------------------|
| **Hosted by**      | `methods` list object         |
| **Mandatory Keys** | `datatype`                    |
| **Optional keys**  | `range`, `arraysize`, `range` |


The optional `error` element defines an error value to return. The
`error` element is returned in addition to any `out` elements
specified for the method call.

Please see the `methods` sample code above for an example of how
`error` parameter lists are used

If no `error` element is specified, no specific error code is
returned. Results may still be returned as an `out` parameter`

**Note** `error` specifies return values for the method call
itself. Transport-layer issues arising from interrupted communication,
services going down, etc, are handled on a language-binding level
where each langauage library implements their own way of detecting,
reporting, and recovering from network-related errors.

#### Error parameter key-value: `datatype`
|                  |                                 |
|:-----------------|:--------------------------------|
| **YAML Type**    | string                          |
| **Mandatory**    | Yes                             |
| **Lark grammar** | `"."? CNAME ("." CNAME)* "[]"?` |

Specifies the data type of the returned error value,

The type can be either a native or defined type.

If `datatype` refers to a defined type, this type can be a
local, nested, or externally defined reference.

If the type is an array (ending with `[]`), the `arraysize` key can
optionally be provided to specify the number of elements in the array.

If `arraysize` is not specified for an array type, the member array
can contain an arbitrary number of elements.


#### Error parameter key-value: `arraysize`
|                  |                    |
|:-----------------|:-------------------|
| **YAML Type**    | int                |
| **Mandatory**    | No                 |
| **Lark grammar** | [positive integer] |

Specifies the number of elements in the input parameter array.

This key is only allowed if the `datatype` element specifies an array
(ending with `[]`).

#### Error parameter key-value: `range`
|                  |                             |
|:-----------------|:----------------------------|
| **YAML Type**    | string                      |
| **Mandatory**    | No                          |
| **Lark grammar** | [See separate grammar file] |


Specifies the legal range for the value.

Please see [value range specification](#value-range-specification)
chapter for details on how to specify ranges.


----------------------

## Namespace list object: `events`
|                        |                   |
|:-----------------------|:------------------|
| **Hosted by**          | `namespaces` list |
| **Mandatory Keys**     | `name`            |
| **Optional keys**      | `description`     |
| **Mand. hosted lists** | N/A               |
| **Opt. hosted lists**  | `in`              |
|                        |                   |

Each `events` list object specifies a fire-and-forget call, executed
by zero or more subscribing instances, that does not return a value.

Execution is best effort to UDP level with server failures not being
reported.

A `events` sample list object is given below:

```
events:
  - name: seat_moving
    description: Signal that the seat has started or stopped moving

    in:
      - name: status
        description: The movement status, moving (1), not moving (0)
        datatype: boolean

      - name: row
        description: The row of the seat
        datatype: uint8

      - name: index
        description: The index of the seat position in the row
        datatype: uint8

      - name: component
        description: The seat component that is moving
        datatype: movement.seat_component_t
```

### Event key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Defines the name of the event.

### Event key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Specifies a description of the event.

### Event list object: `in`
|                    |                                |
|:-------------------|:-------------------------------|
| **Hosted by**      | `methods` list object          |
| **Mandatory Keys** | `name`, `datatype`             |
| **Optional keys**  | `description`,`value`, `range` |

Each `in` list object defines an input parameter to the event

Please see the `events` sample code above for an example of how 
`in` parameter lists are used

#### In parameter key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Specifies the name of the input parameter


#### In parameter key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Contains a description of the input parameter.


#### In parameter key-value: `datatype`
|                  |                                 |
|:-----------------|:--------------------------------|
| **YAML Type**    | string                          |
| **Mandatory**    | Yes                             |
| **Lark grammar** | `"."? CNAME ("." CNAME)* "[]"?` |

Specifies the data type of the input argument,

The type can be either a native or defined type.

If `datatype` refers to a defined type, this type can be a
local, nested, or externally defined reference.

If the type is an array (ending with `[]`), the `arraysize` key can
optionally be provided to specify the number of elements in the array.

If `arraysize` is not specified for an array type, the member array
can contain an arbitrary number of elements.

#### In parameter key-value: `arraysize`
|                  |                    |
|:-----------------|:-------------------|
| **YAML Type**    | int                |
| **Mandatory**    | No                 |
| **Lark grammar** | [positive integer] |

Specifies the number of elements in the input parameter array.

This key is only allowed if the `datatype` element specifies an array
(ending with `[]`).

#### In parameter key-value: `range`
|                  |                             |
|:-----------------|:----------------------------|
| **YAML Type**    | string                      |
| **Mandatory**    | No                          |
| **Lark grammar** | [See separate grammar file] |


Specifies the legal range for the value.

Please see [value range specification](#value-range-specification)
chapter for details on how to specify ranges.


--------------------

## Namespace list object: `properties`

|                        |                            |
|:-----------------------|:---------------------------|
| **Hosted by**          | `namespaces` list          |
| **Mandatory Keys**     | `name`, `datatype` |
| **Optional keys**      | `description`, `range`     |

Each `properties` list object specifies a shared state object that can
be read and set, and which is available to all subscribing entities.

A `properties` sample list object is given below, together with a struct definition:

```
structs
  - name: dome_light_status_t
    description: The dome light status
    members:
      - name: brightness
        description: The brightness of dome light. (0 - off, 255, max brightness)
        datatype: uint8

      - name: red
        description: The amount of red in the dome light color (0 - no, 255 full red)
        datatype: uint8

      - name: green
        description: The amount of green in the dome light color (0 - no, 255 full green)
        datatype: uint8

      - name: blue
        description: The amount of blue in the dome light color (0 - no, 255 full blue)
        datatype: uint8

properties:
  - name: dome_light_status
    description: The dome light status
    datatype: dome_light_status_t

```

### Property key-value: `name`
|                  |         |
|:-----------------|:--------|
| **YAML Type**    | string  |
| **Mandatory**    | Yes     |
| **Lark grammar** | `CNAME` |

Defines the name of the property.

### Property key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

Specifies a description of the property.

### Property key-value: `datatype`
|                  |                                 |
|:-----------------|:--------------------------------|
| **YAML Type**    | string                          |
| **Mandatory**    | Yes                             |
| **Lark grammar** | `"."? CNAME ("." CNAME)* "[]"?` |

Specifies the data type of the property,

The type can be either a native or defined type.

If `datatype` refers to a defined type, this type can be a
local, nested, or externally defined reference.

If the type is an array (ending with `[]`), the `arraysize` key can
optionally be provided to specify the number of elements in the array.

If `arraysize` is not specified for an array type, the member array
can contain an arbitrary number of elements.

### Property key-value: `arraysize`
|                  |                    |
|:-----------------|:-------------------|
| **YAML Type**    | int                |
| **Mandatory**    | No                 |
| **Lark grammar** | [positive integer] |

Specifies the number of elements in the input parameter array.

This key is only allowed if the `datatype` element specifies an array
(ending with `[]`).


### Property parameter key-value: `range`
|                  |                             |
|:-----------------|:----------------------------|
| **YAML Type**    | string                      |
| **Mandatory**    | No                          |
| **Lark grammar** | [See separate grammar file] |


Specifies the legal range for the property.

Please see [value range specification](#value-range-specification)
chapter for details on how to specify ranges.


--------------------

## Namespace list object: `includes`

|                    |                   |
|:-------------------|:------------------|
| **Hosted by**      | `namespaces` list |
| **Mandatory Keys** | `file`            |
| **Optional keys**  | `description`     |

Each `includes` list object specifies a VSC YAML file to be included into the namespace
hosting the `includes` list.

The included file's `structs`, `typedefs`, `enumerators`, `methods`,
`events`, and `properties` lists will be appended to the corresponding
lists in the hosting namespace.

**FIXME:**  Define how we handle overrides / name clashes.

A `includes` sample list object is given below, with global error codes installed under
the `top_level_namespace` namespace that hosts the `includes` list object:

**File: `vsc-error.yml`**

```YAML
name: stdvsc
major-version: 1
minor-version: 0
description: Standard error codes.

enumerations:
  #
  # Error enumeration
  #
  - name: error_t
    type: enumeration

    datatype: int16
    options:
      - name: null
        value: 0
        description: No return value

      - name: ok
        value: 1
        description: No error.

      - name: in_progress
        value: 2
        description: The operation has been initialized and is in progress

      - name: completed
        value: 3
        description: The operation has been completed

      - name: permission_denied
        value: -1
        description: Caller does not have permission to carry out operation

      - name: not_found
        value: -2
        description: A resource or service was not found
```

**File: `hvac_sample.yml`**

```YAML
namespaces:
  - name: top_level_namespace
    includes
    - file: vsc-error.yml;
      description: Global error used by methods in this file

  - name: hvac_namespace
    methods:
      - name: get_hvac_fan_speed
        description: Get the status of the hvac fan.

        out:
          - name: speed
            description: The speed of the fan
            datatype: uint8

          - name: result
            description: The result of the operation
            datatype: .stdvsc.error_t
```

### Include key-value: `file`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | Yes           |
| **Lark grammar** | [YAML string] |

The path to the file to include

### Include key-value: `description`
|                  |               |
|:-----------------|:--------------|
| **YAML Type**    | string        |
| **Mandatory**    | No            |
| **Lark grammar** | [YAML string] |

A description of the include directive

