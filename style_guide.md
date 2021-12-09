# Code Style Guide

## Purpose

The intent of this document is to establish a shared set of rules and guidelines for 
code style, including naming, formatting, and other conventions found in Salesforce development.

Good style guides list rules and standards. Great style guides help establish 
the engineering culture of the organization. As such, this document should be a living
representation of HigherEchelon's shared beliefs regarding code style and quality.

---

## Naming

### Class

- Use Pascal Case
  - AccountTriggerHandler
- Convention for naming classes is <SObject><Trigger (opt.)><Handler|HandlerHelper|Service|Controller|Selector|etc.>
- Avoid abbreviations, underscores, or acronyms
- When in doubt, aim for clarity over brevity. Code is free to write, but hard to understand.

|                        | Suffix               | Example                     |
|------------------------|----------------------|-----------------------------|
| Trigger                | Trigger              | AccountTrigger              |
| Trigger Handler        | TriggerHandler       | AccountTriggerHandler       |
| Trigger Handler Helper | TriggerHandlerHelper | AccountTriggerHandlerHelper |
| Service                | Service              | AccountService              |
| Selector / Query       | Selector             | AccountSelector             |
| Utility                | Util                 | FilterUtil                  |
| VF Controller          | Controller           | AccountMatcherController    |
| Aura Controller        | AuraService          | AccountAuraService          |
| Model / POJO           | (no suffix)          | Book.cls                    |

  
### Variable

- Use Camel Case for local variables and class properties
  - queriedAccountRecords
- Use capital snake case for constants
  - SOQL_LIMIT_SYNC
- Abbreviations should be used for local variables with limited scope
  - The more lines of code between the variable declaration and last use, the more descriptive the name should be
  - accsForReq (accountsForCalloutRequest)
- Never use single letter variables outside of loop iterators
- Aim for uniformity in abbreviation patterns, considering other code in codebase, file, and method.
  - Decrease the cognitive load of the reader, they appreciate clarity and simplicity over fancy naming models

