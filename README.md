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


# Scripting
## Logging
Always include logger definition as the first line of a script module, or a script transform. Loggers should be defined with `system.util.getLogger()` and use the full package path as their name.

i.e. for a script located at `my_folder/my_script` the logger would be defined as:
```
logger = system.util.getLogger("my_folder.my_script")
```

Once the logger is defined, a message may be sent to the Gateway log using one of several functions. It can be found under Status > Logs on the Gateway page. Several options for different types of messages are described in the next section.
```python
logger.debug('This is a debug message')
```

When actively developing a script in Ignition, it is often more convenient to use the print() function to write debug messages to Ignition's console instead of logging messages that have to be checked on the Gateway. 

Once the script is in use, calls to print() for debugging should be removed and loggers should be put in place to cover any necessary debugging points and error messages.

#### Logging Levels
When sending messages to the Ignition logger, there are different levels of severity that can be attached to the log. The most common are Info, Error, Debug, and Trace.


**Info:** The standard log level indicating that something happened, the application entered a certain state, etc. These messages do not require a response, but notify users of the progress of everyday processes.
```python
logger.info('Successful connection to database.')
```

**Error:** This level is used for messages that are intended to be seen by end users, so they should not contain too much technical language that would only be useful in debugging. An Error message should be logged when an error occurs as a result of a user interacting with the application. This way the user can confirm that something is wrong instead of seeing the interaction fail silently.
```python
logger.error('An error occurred while attempting to update equipment mode.')
```
**Debug:** This level should be used for diagnosing and troubleshooting general issues within the application. These can be left in the code during normal system operation to flag when an unexpected error has occurred.
```python
logger.debug('Error in function PrintMyArray: invalid data type, expected array')
```

**Trace:** The most specific type of log. As the name suggests, this is used when you want to trace through every step of your script to determine exactly how a request is being processed or data is being generated. These should be added while you are actively debugging the process, and not left to run during normal system operation.
```python
logger.trace('Loop %s, current equipment is %s, current state is %s' % (counter, equipName, equipState))
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
def csvToDataSet(fileData, hasHeaders):
  """
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

## Folder Structure
The top-level Views folder should contain folders that organize the views based on the navigation structure of the application. The steps taken to access a view in the Designer should be similar to those taken to access the view in the application.

Each distinct view that can be accessed by a user should have its own folder. This folder should contain one view, named Overview, and another folder named Embedded Views. Any embedded views used by the Overview should be stored in this Embedded Views folder; in turn, these embedded views may be composed of their own embedded views, so the structure will repeat for as many folder levels deep as is necessary.

## View Structure
The root container of a view should be one of the following:

**Flex Container:** This should be the default for almost all views. Building a view inside of a flex container allows the view to be responsive to different screen sizes so that it may be designed for TVs, computer screens, mobile devices, and anything in between.

**Breakpoint Container:** More situational than the flex container, this type should be used when you need to render the same view in a different way depending on the screen size. If you need to replace text with icons on a smaller screen, or add additional components on a larger screen, use this container.

**Coordinate Container:** This should be the least commonly used container. It is appropriate when designing fixed-point P&ID style screens that are not intended to be viewed on various screen sizes. If the exact location and size of the components in the view is necessary to understand their meanings, this container may be used.

When developing views, prioritize scalability and the ability to reuse the view elsewhere in the application. If the view will be embedded in another view, as much data as possible should be passed in as view parameters rather than being generated from within the view.

## Bindings

Component properties should not be bound directly to other component properties.

Generally, component properties should be bound to view parameters or custom properties on the view. That way, the binding will remain valid even if components are moved or renamed.

If a component property needs to be accessed by another component, it should be stored in a custom property on the view with a bidirectional binding or change script.

