# Overview
This is an opinionated document built with the intention of creating consistent and easy to manage Ignition projects.


# Naming
Function names, variable names, and filenames should be descriptive; eschew abbreviation. In particular, do not use abbreviations that are ambiguous or unfamiliar to readers outside your project, and do not abbreviate by deleting letters within a word. All Pythonic naming is derived from [Guido](https://en.wikipedia.org/wiki/Guido_van_Rossum)'s recommendations.

## Names to avoid
- single character names, except for specifically allowed cases:
  - counters or iterators (e.g. `i`, `j`, `k`, `v`, et al.)
  - `e` as an exception identifier in try/except statements.
  - `f` as a file handle in with statements


  Please be mindful not to abuse single-character naming. Generally speaking, descriptiveness should be proportional to the name’s scope of visibility. For example, i might be a fine name for 5-line code block but within multiple nested scopes, it is likely too vague.

- dashes (`-`) in any package/module name

- `__double_leading_and_trailing_underscore__` names (reserved by Python)

- offensive terms

- names that needlessly include the type of the variable (for example: `id_to_name_dict`)

## Scripting
`module_name`, 
`package_name`, 
`ClassName`, 
`method_name`, 
`ExceptionName`, 
`function_name`,
`GLOBAL_CONSTANT_NAME`,
`global_var_name`, 
`instance_var_name`,
`function_parameter_name`, 
`local_var_name`,
## Perspective
`folderName`,
`viewName`,
`PageName`,
`propertyName`,
`messageHandlerName`,
`componentName`,
## Vision
`folderName`,
`templateName`,
`componentName`,
`propertyName`,
`parameterName`,

## Tags
`Folder Name`,
`Udt Type Name`,
`Tag Name`,
`parameterName`,

## Stored Procedures
`SCHEMA_NAME`,
`DATABASE_NAME`,
`storedProcedureName`,
`TableName`,
`vVIEW_NAME`,
`STANDARD SYNTAX`,
`functionName`,

# Architecture
## Visualization
## Config

# Scripting
## Project Structure
## Module Structure
## Logging
Always include logger definition as the first line of a script module, or a script transform. Loggers should be defined with `system.util.getLogger()` and use the full package path as their name.

i.e. for a script located at `my_folder/my_script` the logger would be defined as:
```
logger = system.util.getLogger("my_folder.my_script")
```

## Comments
Functions should be documented with a standard header in the following format. The header should be written as a docstring.
```
Description: [Brief summary of what the function is used for]
Parameters: [A list of parameters taken by this function. Should include the expected data type and whether the parameter is required or optional.]
Revisions: [A new note should be added every time the function is changed, with date and initials so later readers know who to ask about the function.] 
```
Example:
```python
"""
NAME: csvToDataSet
DESCRIPTION: Given file data from an imported CSV file, return a dataset suitable for display in an Ignition table. 
            This function is compatible with data accessed by the Perspective File Upload component.
PARAMETERS: fileData (REQ, array) - the bytes of a selected file. Obtained by file.getBytes().
            hasHeaders (REQ, boolean) - True if the spreadsheet has a header row that should not be included in the actual dataset 
REVISIONS:
           *WS 10/21 - Created
           *WS 11/21 - Improved error handling
"""
```

## Function Length
Prefer small and focused functions.

We recognize that long functions are sometimes appropriate, so no hard limit is placed on function length. If a function exceeds about 40 lines, think about whether it can be broken up without harming the structure of the program.

Even if your long function works perfectly now, someone modifying it in a few months may add new behavior. This could result in bugs that are hard to find. Keeping your functions short and simple makes it easier for other people to read and modify your code.

You could find long and complicated functions when working with some code. Do not be intimidated by modifying existing code: if working with such a function proves to be difficult, you find that errors are hard to debug, or you want to use a piece of it in several different contexts, consider breaking up the function into smaller and more manageable pieces.

# Perspective
## Project Structure
## View Structure
## Bindings

Component properties should not be bound directly to other component properties.

Generally, component properties should be bound to view parameters or custom properties on the view. That way, the binding will remain valid even if components are moved or renamed.

If a component property needs to be accessed by another component, it should be stored in a custom property on the view with a bidirectional binding or change script.

## Logging

# Vision
## Project Structure
## Template Structure
## Bindings


## Logging

# Named Queries
## Project Structure
## Stored Procedures

# SQL
## Stored Procedures

# Tags
## Parameter Inheritance
## Tag Scripts
## OPC Paths

# Debugging
## Non-Invasive Debugging

# Error Handling
## Creating an Exception
## Value add exception information

# Version Control
## Git
## Github



