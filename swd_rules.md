# Metadata specification for `.swd` files
The aim of this document is to define the metadata tags needed to document rules in `.swd` files, so they can be included in the ontology referencing them. 

## Background and assumptions

A rule document is a `.swd` file extension with rules on it. A rule has an antecedent and a consequent, as shown below:

```
import <http://example.org/conversion.owl> as conv.

rule rule_name
if  ?variable =<"0"^^xsd:decimal
    and conv:function(?variable2, ?variable, 1) 
then function2(?variable , ?variable2)
```

The `import` command shows the ontology to import. Everything before the `then` keyword is considered the rule antecedent, while everything after the `then` keyword is considered the rule consequent. There are no multiple `if` statements under the same rule.

A rule may have comments, which may  be inline or span multiple lines. 
* Single line comment:
```
# this is a comment
rule rule_name
if  ?variable =<"0"^^xsd:decimal
    and conv:function(?variable2, ?variable, 1) 
then function2(?variable , ?variable2)
```

- Multiple line comments: 
```
rule rule_name
"""
Multiple 
line 
comment
"""
if  ?variable =<"0"^^xsd:decimal
    and conv:function(?variable2, ?variable, 1) 
then function2(?variable , ?variable2)
```
Multiple line comments syntax may also be applied in a single line:

```
rule rule_name
""" Multiple  line  comment using a single line """
if  ?variable =<"0"^^xsd:decimal
    and conv:function(?variable2, ?variable, 1) 
then function2(?variable , ?variable2)
```

**Feedback needed**: Is `"""` acceptable? we need it to determine how to create the parser. I need to make sure it is compatible with the programming synatx used (I have assumed python-like)

## Rule metadata specifications
Rules can be describled with metadata, as shown below. To document a rule, a multi-line comment should be placed **below** the rule id. Metadata fields must be declared starting with the character '`@`'.

- **rule id** [mandatory]: Rule identifier within the `swd` program. It is next to the `rule` keyword.
- **rule label** [optional]: A human readable label for a rule. If none are provided, the `rule id` will be used. It uses the tag `@label`.
- **rule description** [optional]: A human readable description of a rule. It uses the tag `@description`.
- **rule group** [optional]: Mechanism to group rules, which are identified with ids. It uses the tag `@group`. Group ids must be written without spaces and special characters. 

## Example
Below there is an example documenting the simple rule above.

```
rule rule_name
""" 
    @label: only a single line
    @description: long description about what this rule does.
    It may span multiple lines
    @group: only_one_line
"""
if  ?variable =<"0"^^xsd:decimal
    # this comment would be ignored
    and conv:function(?variable2, ?variable, 1) 
then function2(?variable , ?variable2)
"""
This comment
would be ignored
"""
```
This would extract the following information:
- rule: `rule_name`
- rule label: `only a single line`
- rule description: `long description about what this rule does.
    It may span multiple lines`
- rule group: `only_one_line`



