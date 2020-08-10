# OWASP Rules for Stored XSS Prevention
Adapted from the [Official OWASP cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

## RULE #0 - Never Insert Untrusted Data Except in Allowed Locations
The first rule is to deny all - don't put untrusted data into your HTML document unless it is within one of the slots defined in Rule #1 through Rule #5.

## RULE #1 - HTML Escape Before Inserting Untrusted Data into HTML Element Content¶

  - Use double brackets where possible, as they are html-escaped automatically
  - If html tags are needed in the output, use double brackets and mark HTML safe with Ember.String.htmlSafe.    You should only do that after you have verified that the content you mark as safe cannot be injected by a malicious user.
  - Avoid triple brackets
## RULE #2 - Attribute Escape Before Inserting Untrusted Data into HTML Common Attributes

  - Use double brackets where possible to leverage built-in HTML escaping
## RULE #3 - JavaScript Escape Before Inserting Untrusted Data into JavaScript Data Values

- Handelbars does not escape JavaScript strings
## RULE #4 - CSS Escape And Strictly Validate Before Inserting Untrusted Data into HTML Style Property Values

  - Rule #4 is for when you want to put untrusted data into a stylesheet or a style tag. CSS is surprisingly powerful, and can be used for numerous attacks. Therefore, it's important that you only use untrusted data in a property value and not into other places in style data. You should stay away from putting untrusted data into complex properties like url, behavior, and custom (-moz-binding).
  - Please note there are some CSS contexts that can never safely use untrusted data as input - EVEN IF PROPERLY CSS ESCAPED! You will have to ensure that URLs only start with http not javascript and that properties never start with "expression".

## RULE #5 - URL Escape Before Inserting Untrusted Data into HTML URL Parameter Values

  - Rule #5 is for when you want to put untrusted data into HTTP GET parameter value.
  - Use double brackets to leverage built-in HTML / attribute escaping

## RULE #6 - Sanitize HTML Markup with a Library Designed for the Job

  If your application handles markup -- untrusted input that is supposed to contain HTML -- it can be very difficult to validate. Encoding is also difficult, since it would break all the tags that are supposed to be in the input. Therefore, you need a library that can parse and clean HTML formatted text. There are several available at OWASP that are simple to use:
  - JS / Ember — HTML sanitizer from Google Closure Library
  - Rails — The SanitizeHelper module provides a set of methods for scrubbing text of undesired HTML elements.

## RULE #7 - Avoid JavaScript URL’s

  - Untrusted URL's that include the protocol javascript: will execute javascript code when used in URL DOM locations such as anchor tag HREF attributes or iFrame src locations. Be sure to validate all untrusted URL's to ensure they only contain safe schemes such as HTTPS.
