# Code Style Guide

## Purpose

The intent of this document is to establish a shared set of rules and guidelines for
code style, including naming, formatting, and other conventions found in Salesforce development.

---

## Naming

### Class

- Use Pascal Case
  - AccountTriggerHandler
- Convention for naming classes is <SObject><Trigger (opt.)><Handler|HandlerHelper|Service|Controller|Selector|etc.>
- Avoid abbreviations, underscores, or acronyms
- When in doubt, aim for clarity over brevity. Code is free to write, but hard to understand.

|                        | Suffix               | Example                     |
| ---------------------- | -------------------- | --------------------------- |
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
- Prefix booleans with is-
  - isClosing, isAccountOpen
- If including the collection type, use as a prefix to mimc hungarian notation (map-, lst-, set-)
  - mapContactsByAccId, lstParentOpps
- Suffix Id with -Id
  - accId, closedContactId

---

## Code Style

### Comments

#### Classes, Methods, and Properties

- Formal code documentation provides clarity for the reader and contribute to the professional appearance of the codebase
- All formal structures (classes, methods, public properties, etc) require comments
- Logic inside methods or properties should not, for the most part, need comments
  - Use good judgement, always side towards over-explanation over the presumption of understanding
- Documenting comments should be written using Javadoc style
  - Start the comment with /\*\* (two asterisks)
  - Classes should contain:
    - Short description (no tag)
    - Author's name and Company Name (@author)
    - Date created (@since)
  - Methods should contain
    - Short description (no tag)
    - Parameter names with short description (@param <parameter name>)
    - Return value with short description (@return <description>)
    - Exceptions thrown with condition (@exception <condition>)
  - Class properties
    - Public
      - Short description (no tag)
    - Private
      - No formal javadoc, only standard comment `//` for clarification
  - Constructor
    - If constructor accepts a parameter, use method comment convention
    - If constructor does not accept a parameter but contains logic in the body, include a short description
    - If constructor does not accept a parameter and does not include logic in the body, do not comment
      - Consider removing the constructor, empty constructors are, by default, automatically generated when the class is compiled
- Recommended Visual Studio Code extension for generating the comment boilerplate is `ApexDox VS Code`
  - https://marketplace.visualstudio.com/items?itemName=PeterWeinberg.apexdox-vs-code

#### Logic Comments

- Comments describing logic should explain **why** instead of **what**
  - Consistently following this style guide should ensure clear reading quality
  - Code already tells the reader **what** is going on, not the developer/writer's intent
  - This helps prevent long and winding debugging sessions where "magic code" manages multiple edge cases

### Formatting

#### Spacing

- One space between open bracket and preceeding character
  - Good: `class AccountValidator {`
  - Bad: `class AccountValidator{`
- No spaces between parenthesis and preceeding characters, and parameters
  - Good: `if (name == 'Test') { `
  - Bad: `if( name =='Test' ){`
- One space between property getter/setter brackets and blocks
  - `public Boolean isBypass { get; set; }`

#### If Block

- Two or less conditions in if blocks should be on same line
- Three or more conditions or those that take up more than 100 characters in line should be separated at the condition

#### Example

```java

/**
 * Validation handler for Account object
 * @author Austin Barwick, HigherEchelon, Inc.
 * @since 12/15/2021
 */
public class AccountValidator {

    /**
     * Flag to skip validations in context
     */
    public Boolean isBypass { get; set; }

    private String name;

    /**
     * Constructor
     * @param name Account name
     */
    public AccountHandler(String name) {
        this.name = name;
    }

    /**
     * Formats the name prefixed with a greeting
     * @param greeting Greeting to prefix
     * @return String of the phrase
     */
    public String getNameWithGreeting(String greeting) {
      return greeting + ' ' + this.name;
    }

    /**
     * Checks the account name and test context
     * @return True if the account is not a test record or in a test context
     * @exception AccountValidationException Throws if name is null
     */
    public Boolean validateRecord() {
        if (this.name == null) {
          throw new AccountValidationException();
        }

        // Because of org constraints, test data may exist in production
        // environments and should not be validated
        if (!this.name.contains('test')
            && !this.name.contains('dummy')
            || (this.isBypass && Flags.isTesting())
        ) {
          return true;
        }
        return false;
    }

    /**
     * Custom exception thrown in AccountValidator class
     */
    public class AccountValidationException extends Exception {}
}

```
