# Basics — Canonical Ruleset
 
The attached <output-file schema / requirements file / response schema (XSD)>
are authoritative for column lists, format constraints, and prompt-XML aliases.
Where the rules below and an attached file overlap, defer to the attached file.
 
## 1. Error Messages 
- The Error messages MUST always be output with Add Integration Message
- Add Integration Message Severity for Error Messages MUST be ERROR
- Add Integration Message Sumamry MUST start with "Oh no!". It SHOULD contain only high level information about what happened.

## 2. Data extraction from JSON
- The data MUST be extracted from JSON using the function relevant to the data type
- The data MUST be extracted with a function that provides a default value, if available
- If a function is available, the default SHOULD be:
-- "NO VALUE!" for Strings
-- -1 for Numbers
-- false for Booleans