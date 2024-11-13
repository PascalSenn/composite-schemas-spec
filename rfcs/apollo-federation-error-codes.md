# Validation Rule from Federation

## Implemented

| Status | Error                              | Description                                                                                                                                                         |
| ------ | ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ✅     | DISALLOWED_INACCESSIBLE            | An element is marked as @inaccessible that is not allowed to be @inaccessible.                                                                                      |
| ✅     | EMPTY_MERGED_ENUM_TYPE             | An enum type has no value common to all the subgraphs that define the type. Merging that type would result in an invalid empty enum type.                           |
| ✅     | EMPTY_MERGED_INPUT_TYPE            | An input object type has no field common to all the subgraphs that define the type. Merging that type would result in an invalid empty input object type.           |
| ✅     | EXTERNAL_ARGUMENT_DEFAULT_MISMATCH | An @external field declares an argument with a default that is incompatible with the corresponding argument in the declaration(s) of that field in other subgraphs. |
| ✅     | EXTERNAL_ARGUMENT_MISSING          | An @external field is missing some arguments present in the declaration(s) of that field in other subgraphs.                                                        |
| ✅     | EXTERNAL_ARGUMENT_TYPE_MISMATCH    | An @external field declares an argument with a type that is incompatible with the corresponding argument in the declaration(s) of that field in other subgraphs.    |
| ✅     | EXTERNAL_MISSING_ON_BASE           | A field is marked as @external in a subgraph but with no non-external declaration in any other subgraph.                                                            |
| ✅     | EXTERNAL_TYPE_MISMATCH             | An @external field has a type that is incompatible with the declaration(s) of that field in other subgraphs.                                                        |
| ✅     | EXTERNAL_UNUSED                    | An @external field is not being used by any instance of @key, @requires, @provides or to satisfy an interface implementation.                                       |
| ✅     | FIELD_ARGUMENT_DEFAULT_MISMATCH    | An argument (of a field/directive) has a default value that is incompatible with that of other declarations of that same argument in other subgraphs.               |
| ✅     | FIELD_ARGUMENT_TYPE_MISMATCH       | An argument (of a field/directive) has a type that is incompatible with that of other declarations of that same argument in other subgraphs.                        |
| ✅     | FIELD_TYPE_MISMATCH                | A field has a type that is incompatible with other declarations of that field in other subgraphs.                                                                   |
| ✅     | INPUT_FIELD_DEFAULT_MISMATCH       | An input field has a default value that is incompatible with other declarations of that field in other subgraphs.                                                   |

## Covered by other rule

| Status | Error                                         | Description                                                                                                                                                                        |
| ------ | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ◉      | DEFAULT_VALUE_USES_INACCESSIBLE               | An element is marked as @inaccessible but is used in the default value of an element visible in the API schema.                                                                    |
| ◉      | ENUM_VALUE_MISMATCH                           | An input object type has no field common to all the subgraphs that define the type. Merging that type would result in an invalid empty input object type.                          |
| ◉      | REFERENCE_INACCESSIBLE                        | An element is marked as @inaccessible but is referenced by an element visible in the API schema.                                                                                   |
| ◉      | REQUIRED_ARGUMENT_MISSING_IN_SOME_SUBGRAPH    | An argument of a field or directive definition is mandatory in some subgraphs, but the argument is not defined in all the subgraphs that define the field or directive definition. |
| ◉      | REQUIRED_INACCESSIBLE                         | An element is marked as @inaccessible but is required by an element visible in the API schema.                                                                                     |
| ◉      | REQUIRED_INPUT_FIELD_MISSING_IN_SOME_SUBGRAPH | A field of an input object type is mandatory in some subgraphs, but the field is not defined in all the subgraphs that define the input object type.                               |

## Missing

| Status | Error                                     | Description                                                                                                                                                                                                                       |
| ------ | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 📋     | IMPLEMENTED_BY_INACCESSIBLE               | An element is marked as @inaccessible but implements an element visible in the API schema.                                                                                                                                        |
| 📋     | INTERFACE_FIELD_NO_IMPLEM                 | After subgraph merging, an implementation is missing a field of one of the interfaces it implements (which can happen for valid subgraphs).                                                                                       |
| 📋     | INTERFACE_KEY_MISSING_IMPLEMENTATION_TYPE | A subgraph has a @key on an interface type, but that subgraph does not define an implementation (in the supergraph) of that interface.                                                                                            |
| 📋     | INTERFACE_KEY_NOT_ON_IMPLEMENTATION       | A @key is defined on an interface type, but is not defined (or is not resolvable) on at least one of the interface implementations.                                                                                               |
| 📋     | INVALID_FIELD_SHARING                     | A field that is non-shareable in at least one subgraph is resolved by multiple subgraphs.                                                                                                                                         |
| 📋     | INVALID_SHAREABLE_USAGE                   | The @shareable Federation directive is used in an invalid way.                                                                                                                                                                    |
| 📋     | KEY_DIRECTIVE_IN_FIELDS_ARG               | The fields argument of a @key directive includes some directive applications. This is not supported.                                                                                                                              |
| 📋     | KEY_FIELDS_HAS_ARGS                       | The fields argument of a @key directive includes a field defined with arguments (which is not currently supported).                                                                                                               |
| 📋     | KEY_FIELDS_SELECT_INVALID_TYPE            | The fields argument of @key directive includes a field whose type is a list, interface, or union type. Fields of these types cannot be part of a @key.                                                                            |
| 📋     | KEY_INVALID_FIELDS                        | The fields argument of a @key directive is invalid (it has invalid syntax, includes unknown fields, ...).                                                                                                                         |
| 📋     | NO_QUERIES                                | None of the composed subgraphs expose any query.                                                                                                                                                                                  |
| 📋     | ONLY_INACCESSIBLE_CHILDREN                | A type visible in the API schema has only @inaccessible children.                                                                                                                                                                 |
| 📋     | PROVIDES_DIRECTIVE_IN_FIELDS_ARG          | The fields argument of a @provides directive includes some directive applications. This is not supported.                                                                                                                         |
| 📋     | PROVIDES_FIELDS_HAS_ARGS                  | The fields argument of a @provides directive includes a field defined with arguments (which is not currently supported).                                                                                                          |
| 📋     | PROVIDES_FIELDS_MISSING_EXTERNAL          | The fields argument of a @provides directive includes a field that is not marked as @external.                                                                                                                                    |
| 📋     | QUERY_ROOT_TYPE_INACCESSIBLE              | An element is marked as @inaccessible but is the query root type, which must be visible in the API schema.                                                                                                                        |
| 📋     | REQUIRES_DIRECTIVE_IN_FIELDS_ARG          | The fields argument of a @requires directive includes some directive applications. This is not supported.                                                                                                                         |
| 📋     | REQUIRES_INVALID_FIELDS_TYPE              | The value passed to the fields argument of a @requires directive is not a string.                                                                                                                                                 |
| 📋     | REQUIRES_INVALID_FIELDS                   | The fields argument of a @requires directive is invalid (it has invalid syntax, includes unknown fields, ...).                                                                                                                    |
| 📋     | ROOT_MUTATION_USED                        | A subgraph's schema defines a type with the name mutation, while also specifying a different type name as the root query object. This is not allowed.                                                                             |
| 📋     | ROOT_QUERY_USED                           | A subgraph's schema defines a type with the name query, while also specifying a different type name as the root query object. This is not allowed.                                                                                |
| 📋     | ROOT_SUBSCRIPTION_USED                    | A subgraph's schema defines a type with the name subscription, while also specifying a different type name as the root query object. This is not allowed.                                                                         |
| 📋     | SATISFIABILITY_ERROR                      | Subgraphs can be merged, but the resulting supergraph API would have queries that cannot be satisfied by those subgraphs.                                                                                                         |
| 📋     | SHAREABLE_HAS_MISMATCHED_RUNTIME_TYPES    | A shareable field return type has mismatched possible runtime types in the subgraphs in which the field is declared. As shared fields must resolve the same way in all subgraphs, this is almost surely a mistake.                |
| 📋     | TYPE_DEFINITION_INVALID                   | A built-in or Federation type has an invalid definition in the schema.                                                                                                                                                            |
| 📋     | TYPE_KIND_MISMATCH                        | A type has the same name in different subgraphs, but a different kind. For instance, one definition is an object type but another is an interface.Replaces VALUE_TYPE_KIND_MISMATCH, EXTENSION_OF_WRONG_KIND, ENUM_MISMATCH_TYPE. |
| 📋     | PROVIDES_INVALID_FIELDS                   | The fields argument of a @provides directive is invalid (it has invalid syntax, includes unknown fields, ...).                                                                                                                    |
| 📋     | INVALID_GRAPHQL                           | A schema is invalid GraphQL: it violates one of the rules of the specification.                                                                                                                                                   |

## Later

| Status | Error                                     | Description                                                                                                                           |
| ------ | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| ⏭️     | DIRECTIVE_COMPOSITION_ERROR               | Error when composing custom directives.                                                                                               |
| ⏭️     | DIRECTIVE_DEFINITION_INVALID              | A built-in or Federation directive has an invalid definition in the schema.                                                           |
| ⏭️     | DOWNSTREAM_SERVICE_ERROR                  | Indicates an error in a subgraph service query during query execution in a                                                            |
| ⏭️     | EXTENSION_WITH_NO_BASE                    | A subgraph is attempting to extend a type that is not originally defined in any known subgraph.                                       |
| ⏭️     | KEY_UNSUPPORTED_ON_INTERFACE              | A @key directive is used on an interface, which is only supported when @linking to Federation v2.3 or later.                          |
| ⏭️     | OVERRIDE_COLLISION_WITH_ANOTHER_DIRECTIVE | The @override directive cannot be used on external fields, nor to override fields with either @external, @provides, or @requires.     |
| ⏭️     | OVERRIDE_FROM_SELF_ERROR                  | Field with @override directive has "from" location that references its own subgraph.                                                  |
| ⏭️     | OVERRIDE_LABEL_INVALID                    | The @override directive label argument must match the pattern `/^[a-zA-Z]a-zA-Z0-9\*-:./]\*$/ or /^percent((d{1,2}(.d{1,8})I 100))$/` |
| ⏭️     | OVERRIDE_ON_INTERFACE                     | The @override directive cannot be used on the fields of an interface type.                                                            |
| ⏭️     | OVERRIDE_SOURCE_HAS_OVERRIDE              | Field which is overridden to another subgraph is also marked @override.                                                               |

## Open Questions

| Status | Error                                     | Description                                                                                            |
| ------ | ----------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| ❓     | EXTERNAL_COLLISION_WITH_ANOTHER_DIRECTIVE | The @external directive collides with other directives in some situations.                             |
| ❓     | INTERFACE_OBJECT_USAGE_ERROR              | Error in the usage of the @interfaceObject directive.                                                  |
| ❓     | INVALID_FEDERATION_SUPERGRAPH             | Indicates that a schema provided for an Apollo Federation supergraph is not a valid supergraph schema. |
| ❓     | KEY_INVALID_FIELDS_TYPE                   | The value passed to the fields argument of a @key directive is not a string.                           |
| ❓     | MERGED_DIRECTIVE_APPLICATION_ON_EXTERNAL  | In a subgraph, a field is both marked @external and has a merged directive applied to it.              |
| ❓     | PROVIDES_INVALID_FIELDS_TYPE              | The value passed to the fields argument of a @provides directive is not a string.                      |
| ❓     | PROVIDES_ON_NON_OBJECT_FIELD              | A @provides directive is used to mark a field whose base type is not an object type.                   |
| ❓     | PROVIDES_UNSUPPORTED_ON_INTERFACE         | A @provides directive is used on an interface, which is not (yet) supported.                           |

## Not needed in composite schemas

| Status | Error                             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ------ | --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 🔴     | EXTERNAL_ON_INTERFACE             | The field of an interface type is marked with @external: as external is about marking field not resolved by the subgraph and as interface field are not resolved (only implementations of those fields are), an "external" interface field is nonsensical                                                                                                                                                                                                                                                                  |
| 🔴     | INVALID_LINK_DIRECTIVE_USAGE      | An application of the @link directive is invalid/does not respect the specification.                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 🔴     | INVALID_LINK_IDENTIFIER           | A URL/version for a @link feature is invalid/does not respect the specification.                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 🔴     | INVALID_SUBGRAPH_NAME             | A subgraph name is invalid. (Subgraph names cannot be a single underscore (\*)).                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 🔴     | LINK_IMPORT_NAME_MISMATCH         | The import name for a merged directive (as declared by the relevant @link(import:) argument) is inconsistent between subgraphs.                                                                                                                                                                                                                                                                                                                                                                                            |
| 🔴     | REQUIRES_FIELDS_MISSING_EXTERNAL  | The fields argument of a @requires directive includes a field that is not marked as @external.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 🔴     | REQUIRES_UNSUPPORTED_ON_INTERFACE | A @requires directive is used on an interface, which is not (yet) supported.                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 🔴     | TYPE_WITH_ONLY_UNUSED_EXTERNAL    | A Federation 1 schema has a composite type comprised only of unused external fields. Note that this error can only be raised for Federation 1 schema as Federation 2 schema do not allow unused external fields (and errors with code EXTERNAL_UNUSED will be raised in that case). But when Federation 1 schema are automatically migrated to Federation 2 ones, unused external fields are automatically removed, and in rare case this can leave a type empty. If that happens, an error with this code will be raised. |
| 🔴     | UNKNOWN_FEDERATION_LINK_VERSION   | The version of Federation in a @link directive on the schema is unknown.                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| 🔴     | UNKNOWN_LINK_VERSION              | The version of @link set on the schema is unknown.                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 🔴     | UNSUPPORTED_FEATURE               | Indicates an error due to feature currently unsupported by Federation.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 🔴     | UNSUPPORTED_LINKED_FEATURE        | Indicates that a feature used in a @link is either unsupported or is used with unsupported options.                                                                                                                                                                                                                                                                                                                                                                                                                        |

# Removed codes ( in Apollo Federation)

The following error codes have been removed and are no longer generated by the
most recent version of the @apollo/gateway library:

| Status | Error                                       | Description                                                                                                                                                                                                                                  |
| ------ | ------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 🔴     | DUPLICATE_ENUM_DEFINITION                   | As duplicate enum definitions is invalid GraphQL, this will now be an error with code INVALID_GRAPHQL.                                                                                                                                       |
| 🔴     | DUPLICATE_ENUM_VALUE                        | As duplicate enum values are invalid in GraphQL, this will now be an error with code INVALID_GRAPHQL.                                                                                                                                        |
| 🔴     | DUPLICATE_SCALAR_DEFINITION                 | As duplicate scalar definitions are invalid in GraphQL, this will now be an error with code INVALID_GRAPHQL.                                                                                                                                 |
| 🔴     | ENUM_MISMATCH                               | Subgraph definitions for an enum are now merged by composition.                                                                                                                                                                              |
| 🔴     | EXTERNAL_USED_ON_BASE                       | As there is no type ownership anymore, there is also no particular limitation as to where a field can be external.                                                                                                                           |
| 🔴     | INTERFACE_FIELD_IMPLEM_TYPE_MISMATCH        | This error was thrown by a validation introduced to avoid running into a known runtime bug. Since Federation v2.3, the underlying runtime bug has been addressed and the validation/limitation was no longer necessary and has been removed. |
| 🔴     | KEY_FIELDS_MISSING_EXTERNAL                 | Using @external for key fields is now discouraged, unless the field is truly meant to be external.                                                                                                                                           |
| 🔴     | KEY_FIELDS_MISSING_ON_BASE                  | Keys can now use any field from any other subgraph.                                                                                                                                                                                          |
| 🔴     | KEY_MISSING_ON_BASE                         | Each subgraph is now free to declare a key only if it needs it.                                                                                                                                                                              |
| 🔴     | KEY_NOT_SPECIFIED                           | Each subgraph can declare a key independently of any other subgraph.                                                                                                                                                                         |
| 🔴     | MULTIPLE_KEYS_ON_EXTENSION                  | Every subgraph can have multiple keys, as necessary.                                                                                                                                                                                         |
| 🔴     | NON_REPEATABLE_DIRECTIVE_ARGUMENTS_MISMATCH | Since Federation v2.1.0, the case this error used to cover is now a warning (with code INCONSISTENT_NON_REPEATABLE_DIRECTIVE_ARGUMENTS) instead of an error.                                                                                 |
| 🔴     | PROVIDES_FIELDS_SELECT_INVALID_TYPE         | @provides can now be used on fields of interface, union, and list types.                                                                                                                                                                     |
| 🔴     | PROVIDES_NOT_ON_ENTITY                      | @provides can now be used on any type.                                                                                                                                                                                                       |
| 🔴     | REQUIRES_FIELDS_HAS_ARGS                    | Since Federation v2.1.1, using fields with arguments in a @requires is fully supported.                                                                                                                                                      |
| 🔴     | REQUIRES_FIELDS_MISSING_ON_BASE             | Fields in @requires can now be from any subgraph.                                                                                                                                                                                            |
| 🔴     | REQUIRES_USED_ON_BASE                       | As there is no type ownership anymore, there is also no particular limitation as to which subgraph can use a @requires.                                                                                                                      |
| 🔴     | RESERVED_FIELD_USED                         | This error was previously not correctly enforced: the service and entities, if present, were overridden; this is still the case.                                                                                                             |
| 🔴     | VALUE_TYPE_NO_ENTITY                        | There is no strong difference between entity and value types in the model (they are just usage patterns), and a type can have keys in one subgraph but not another.                                                                          |
| 🔴     | VALUE_TYPE_UNION_TYPES_MISMATCH             | Subgraph definitions for a union are now merged by composition.                                                                                                                                                                              |