```python
'''this is a doc string. It goes at the top. Pylint doesn't like it iif you don't have one'''

# Usually you import at the top of your code
import system 
# To import a specif module use this format. 
from string import ascii_lowercase

# Function names should be lowercase, with words separated by underscores # as necessary to improve readability.
def add_me():
	pause

# How to refer to module import
print(sys.executable)
# If you used the from <> import <> format
print(asci_lowercase)

```


### Naming conventions

The following naming styles are commonly distinguished:

-   `b` (single lowercase letter)
-   `B` (single uppercase letter)
-   `lowercase`
-   `lower_case_with_underscores`
-   `UPPERCASE`
-   `UPPER_CASE_WITH_UNDERSCORES`
-   `CapitalizedWords` (or CapWords, or CamelCase – so named because of the bumpy look of its letters [[4]](https://peps.python.org/pep-0008/#id8)). This is also sometimes known as StudlyCaps.
    
    Note: When using acronyms in CapWords, capitalize all the letters of the acronym. Thus HTTPServerError is better than HttpServerError.
    
-   `mixedCase` (differs from CapitalizedWords by initial lowercase character!)
-   `Capitalized_Words_With_Underscores` (ugly!)

