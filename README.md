# Overview
This is an opinionated document built with the intention of creating consistent and easy to manage Ignition projects.

# Linting
Run pylint over your code using [this pylintrc](.pylintrc).

Advantages and disadvantages of linting can be found in the [Google python styling guide](https://google.github.io/styleguide/pyguide.html).

> pylint is a tool for finding bugs and style problems in Python source code. It finds problems that are typically caught by a compiler for less dynamic languages like C and C++. Because of the dynamic nature of Python, some warnings may be incorrect; however, spurious warnings should be fairly infrequent.

Suppress warnings if they are inappropriate so that other issues are not hidden. To suppress warnings, you can set a line-level comment:

```python
dict = 'something awful'  # Bad Idea... pylint: disable=redefined-builtin
```

`pylint` warnings are each identified by symbolic name (`empty-docstring`).

Suppressing in this way has the advantage that we can easily search for suppressions and revisit them.

You can get a list of pylint warnings by doing:
```sh
pylint --list-msgs
```
To get more information on a particular message, use:
```sh
pylint --help-msg=C6409
```

To execute `pylint` around the entire project a search for all `.py` files can be used like the following:
```sh
find . -name "*.py" | xargs pylint
```

To execute `pylint` around the entire project, and also print out the file path in any errors in a VS code format you can run the following:
```sh
find . -name "*.py" | xargs pylint --msg-template="{path}({line}): [{obj}] {msg} ({symbol})"
```

The following github action can be used to automate linting on any of your repository pull requests
```yaml
name: Pylint

on: [push]

jobs:
  pylint:
    name: Lint Code
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 2.7
      uses: actions/setup-python@v1
      with:
        python-version: 2.7
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pylint
    - name: Run pylint and ensure a perfect score
      run: find . -name '*.py' | xargs pylint;
```


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

## File extensions
- For Yet Another Markup Language (YAML) files needed for files such as for Docker Compose, GitHub Actions, etc. the preferred file extension is `.yaml`.
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
`Folder Name`,
`View Name`,
`PageName`,
`propertyName`,
`messageHandlerName`,
`Component Name`,
## Vision
`Folder Name`,
`Template Name`,
`Component Name`,
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

If your project contains many loggers that send debugging messages to the Gateway, it may be useful to create a custom session property that can be toggled from within the application to enable or disable the loggers. This property can be checked from the scripts that send log messages to determine if the message should be withheld.

As the project grows in scope, the global setting should be replaced with a configuration screen where a user may enable and disable loggers for specific areas of functionality.

For a more convenient script logging experience, you may also want to implement [this python class](https://gist.github.com/keith-gamble/d2d58f0cd43d8117e0c7e4d81296cb65), which replaces the built-in script logging functions with versions that will also print the logged message to the Scripting Console or Perspective Console as appropriate.

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
```
Example:
```python
def csvToDataSet(file_data, has_headers):
  """
  DESCRIPTION: Given file data from an imported CSV file, return a dataset suitable for display in an Ignition table. 
              This function is compatible with data accessed by the Perspective File Upload component.
  PARAMETERS: file_data (REQ, array) - the bytes of a selected file. Obtained by file.getBytes().
              has_headers (REQ, boolean) - True if the spreadsheet has a header row that should not be included in the actual dataset 
  """
```

If there are no parameters, we should still include the text `None` like so:
```
  PARAMETERS: None
```

Revisions are not needed, as we can use Git-blame to identify who made changes and what the commits and reasoning was.

## Function Length
Prefer small and focused functions.

We recognize that long functions are sometimes appropriate, so no hard limit is placed on function length. If a function exceeds about 40 lines, think about whether it can be broken up without harming the structure of the program.

Even if your long function works perfectly now, someone modifying it in a few months may add new behavior. This could result in bugs that are hard to find. Keeping your functions short and simple makes it easier for other people to read and modify your code.

You could find long and complicated functions when working with some code. Do not be intimidated by modifying existing code: if working with such a function proves to be difficult, you find that errors are hard to debug, or you want to use a piece of it in several different contexts, consider breaking up the function into smaller and more manageable pieces.

# Perspective

## Folder Structure
The top-level native Perspective `Views` folder should contain folders that organize the views based on the navigation structure of the application, and higher level application features. The steps taken to access a view in the Designer should be similar to those taken to access the view in the application.

Each folder under the native Perspective `Views` folder should have the following structure: 
1. One view, with a clear and full-text name that explains the purpose of the view and matches the name of its parent folder.
2. A folder named Embedded Views. Any embedded or repeated views used by the main view should be stored in this Embedded Views folder. This folder may contain multiple views and/or multiple folders to group and organize related or deeply nested views.
3. (Optional) A folder named Popups containing all popup views.

For Example:

```sh
└── Example Feature
    ├── Embedded Views
    │   ├── Messenger
    │   │   ├── Sent Messages
    │   │   │   ├── Sent Messages 1
    │   │   │   └── Sent Messages 2
    │   │   ├── Received Messages
    │   │   │   ├── Received Messages 1
    │   │   │   └── Received Messages 2
    │   │   ├── Subview 1
    │   │   └── Subview 2
    │   ├── Shopping
    │   │   ├── Shopping 1
    │   │   ├── Shopping 2
    │   │   └── Shopping 3
    │   ├── Subview 1
    │   └── Subview 2
    ├── Popups
    │   ├── Error
    │   └── Warning
    └── Example Feature
```

The top level native Perspective `Views` folder contains a folder that describes the primary feature in this tree. \
The `Example Feature` folder contains the `Embedded Views` folder, `Popups` folder, and `Example Feature` view.\
The `Embedded Views` folder expands into multiple folders and views, the folders organize related views that need to stay together.\
The `Shopping` folder simply contains a few related views, while the `Messenger` folder contains a subviews and additional folders to further group and organize nested views.

## View Structure
The root container of a view should be one of the following:

**Flex Container:** This should be the default for almost all views. Building a view inside of a flex container allows its components to grow, shrink, and wrap in response to the size of the screen on which it is rendered. This way, the same view may be designed for TVs, computer screens, mobile devices, and anything in between.

**Breakpoint Container:** More situational than the flex container, this type should be used when you need to render the same view in a different way depending on the screen size. If you need to replace text with icons on a smaller screen, or add additional components on a larger screen, use this container.

**Coordinate Container:** This should be the least commonly used container. It is appropriate when designing fixed-point P&ID style screens that are not intended to be viewed on various screen sizes. If the exact location and size of the components in the view is necessary to understand their meanings, this container may be used.

When developing views, prioritize scalability and the ability to reuse the view elsewhere in the application. If the view will be embedded in another view, as much data as possible should be passed in as view parameters rather than being generated from within the view.

## Bindings

Component properties should not be bound directly to other component properties.

Generally, component properties should be bound to view parameters or custom properties on the view. That way, the binding will remain valid even if components are moved or renamed.

If a component property needs to be accessed by another component, it should be stored in a custom property on the view with a bidirectional binding or change script.


# Architecture Visualization

A visual representation of the architecture around the gateway can be useful when planning a project or sharing designs with a client.

![Architecture Diagram](https://inductiveautomation.com/static/images/architectures/ArchitectureDiagram-Standard@2x.png "IA Example Architecture Diagram")

IA provides [a library of graphics](https://bw1.sharepoint.com/sites/IgnitionUsersGroup/Shared%20Documents/Helpful%20Ignition%20Resources/ArchitectureDiagramElements.zip) that represent many different devices that may be included in the architecture. These graphics should be used when building our own diagrams to promote clarity and consistency between projects.

The use of these graphics is subject to the following restrictions:

- To use these graphics, you must be a member in good standing in one of Inductive
Automation’s partner programs (Integrator Program, Ignition Onboard Program,
Distributor Program), or you must have express written permission from Inductive
Automation’s Director of Marketing.

- Architecture diagrams must display the following sentence at a size that is easily legible: 
  ```
  This architecture has not been validated by Inductive Automation.
  ```

If a specific Ignition or third-party module is used to develop functionality for certain devices, that can be represented in the diagram using this [library of module icons](https://bw1.sharepoint.com/sites/IgnitionUsersGroup/Shared%20Documents/Helpful%20Ignition%20Resources/ModuleIconsPack.zip). It contains graphics that represent Ignition's modules, as well as the modules from [Cirrus Link](https://cirrus-link.com/) and [Sepasoft](https://www.sepasoft.com/).

Architecture diagrams should be as simple as possible while still communicating the relevant concepts. It is not necessary that every PLC or client device is represented in the diagram, only that the viewer understands how one PLC or one client in general fits into the overall system.

For more examples of diagrams for different architectures, refer to [Inductive's Common Ignition Architectures page.](https://inductiveautomation.com/ignition/architectures)



## Markdown Syntax
The best comprehensive resouce for Markdown is the [Markdown Guide.](https://www.markdownguide.org/basic-syntax/) Also see the [Extended Syntax](https://www.markdownguide.org/extended-syntax) page for more complex elements like tables and footnotes.

Important elements to know about are:

##### Code Snippets
Place three backticks ( ` ) before and after a section to identify it as a code snippet.

*This code:*

````
```
def myFunction()
  // do stuff
```
````

*will render as the following:*

```
def myFunction()
  // do stuff
```

Any code within a Markdown document should be contained in a snippet.

##### Links
To add a link to a website, write the link text in [brackets], followed by the URL in (parantheses).

*This code:*
```
Check out the [style guide!](https://github.com/design-group/style-guide)
```
*will render as the following:*

Check out the [style guide!](https://github.com/design-group/style-guide)

##### Lists
To create a list, place a dash ( - ) before the line that should begin the list.

Subsequent dashes on the same tab level will add more elements to the list, while dashes on an increased tab level will create a nested list.

*This code:*
```
- Milk
- Eggs
- Fruit
  - Bananas
  - Apples
    - Honeycrisp
  - Oranges
```
*will render as the following:*
- Milk
- Eggs
- Fruit
  - Bananas
  - Apples
    - Honeycrisp
  - Oranges

##### Headings
Place a pound sign ( # ) before a line to identify it as a heading.
You can add up to six pound signs to reduce the emphasis of the heading.

*This code:*
```
# Major Heading

### Minor Heading
```
*will render as the following:*

# Major Heading
###### Minor Heading

## Contributions
A lot of inspiration for this doc came from the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)
