# AusCAT Code Guidelines

## Repositories

The following repositories have been defined to hold the source code for AusCAT:

- [auscat_installation](https://github.com/AustralianCancerDataNetwork/auscat_installation)
- [auscat_etl](https://github.com/AustralianCancerDataNetwork/auscat_etl)
- [auscat_etl_centres](https://github.com/AustralianCancerDataNetwork/auscat_etl_centres)
- [auscat_ontology](https://github.com/AustralianCancerDataNetwork/auscat_ontology)
- [auscat_imaging](https://github.com/AustralianCancerDataNetwork/auscat_imaging)
- [auscat_federated_learning](https://github.com/AustralianCancerDataNetwork/auscat_federated_learning)

Additional repositories may be created and should be discussed in the weekly AusCAT technical meeting prior to being created.

## README

Each repository must contain a `README.md` in the root directory of the repository. At a minimum, this README should contain the sections:

- **Overview:** Description of what kind of functions/code is stored within the repository
- **Structure:** More detailed guide to how code is stored within directory structure of repository
- **Installation**: Instructions on how to install and required libraries/dependencies for this code. If there are any tool(s) found within the repository this section describes to a user how to install these.
- **Getting Started:** Brief guide to how to run the core functionality found within the repository. This could contain example code snippets or command line examples.
- **Contact:** Details of who is responsible for this code repository and how to contact them.

### Branching

The production ready source code is kept in the `main` branch of the repository. This branch is locked on all AusCAT repositories. Code may only be contributed via Pull Requests into this main branch following approval via Peer Review.

## Coding Conventions

Every programming language has standard coding conventions which should be followed. Code within the AusCAT repositories should adopt appropriate coding conventions. For Python code these are defined below, for other programming languages the standards adopted should be clearly defined in the repository's README.

### Python

[PEP-8](https://www.python.org/dev/peps/pep-0008/) is Python's widely adopted coding standard and should be followed by all code within AusCAT repositories. [`pylint`](https://pylint.org/) should be used where possible to enforce these style guides. GitHub actions can run these checks automatically on new Pull Requests to assist the contributor and peer reviewer with enforcing these coding conventions. A `.pylintrc` may be included in the repository to define checks which are deemed excessive (based on discussion in the AusCAT technical meeting).

### Commenting Code

Comments should be used throughout the source code to describe those portions of code where the purpose is not clear from the code itself. Remember to use informative variable and function names to avoid needed excessive comments.

### Doc strings

Every function should be contain a description of what it does, it's input parameters, it's output and what exceptions it might raise. When coding in Python, the Google Doc String format should be used throughout (be sure to add an extension to your favorite code editor to assist with docstring formatting).

#### Google Doc String Format Example

```python
def function_with_types_in_docstring(param1, param2):
    """Example function with types documented in the docstring.

    `PEP 484`_ type annotations are supported. If attribute, parameter, and
    return types are annotated according to `PEP 484`_, they do not need to be
    included in the docstring:

    Args:
        param1 (int): The first parameter.
        param2 (str): The second parameter.

    Returns:
        bool: The return value. True for success, False otherwise.

    .. _PEP 484:
        https://www.python.org/dev/peps/pep-0484/

    """
```

> See [https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html) for more Google Doc string examples.

## Automated Testing

Currently no automated testing is required for these repositories. This will be revisited in mid to late 2022 to address where such automated testing would be useful.

## Contributing

To contribute code to any of the AusCAT repositories, the following steps should be followed:

1. Ensure a GitHub issue is assigned to you before contributing some code to an AusCAT repository. You contribution should address only the feature/bug described in that issue.
2. Clone the repository locally, and create a branch off `main`. No strict rules are in place for naming of branches, but it should describe the feature or change being contributed.
3. Commit your changes to your branch and push these to GitHub.
4. Once ready to contribute these code to the main branch, create a pull request within the GitHub repository. Assign the appropriate reviewer(s) for this pull request. If unsure add those listed in the Contact section of that repositories README.
5. At least one reviewer will complete the Peer Review on the code. If any issues are found they may request changes which should be addressed by the contributor. Once ready the reviewer may approve the changes

> Note: To enable proper peer review of code, please make sure your pull requests aren't too large. As a guide, a pull request should contain a maximum of 500 new or changed lines of code. If your submission is bigger than 500 lines consider breaking this up into multiple Pull Requests. In certain circumstances larger pull requests are acceptable, such as when refactoring code (moving code from one place to another without changing its functionality).

## Peer Review (Pull Request) Checklist

The reviewer of a pull request should provide feedback on each of the following points. When the reviewer is satisfied that the submission has met the criteria they will approve the Pull Request.

1. **Submission scope:** Check that the submission is of a reasonable size. Make sure the submission isn't modifying parts of the code outside the scope of the feature/fix being implemented in this pull request.
2. **Coding conventions:** Are the agreed upon coding conventions followed in this code?
3. **Understandability:** Is the code easy to understand? Are variables and functions named appropriately? Are functions of a reasonable size and performing one task each?
4. **Maintainability:** Are file paths, server IPs configurable? Are constant values decalared at the top of the file (or in a specific constants file)? No hardcoded items should exist within the code.
5. **Documented:** Do Doc Strings exist and are up-to-date? Is the code commented appropriately? Has accompanying documentation been provided and updated?
6. **Security:** Are their any potential security issues within this code? Have all passwords been removed? Are IP addresses etc configurable?
7. **Functionality:** You don't have to actually run the code yourself, but where possible read over the code and note any potential issues where it may not function as intended.
