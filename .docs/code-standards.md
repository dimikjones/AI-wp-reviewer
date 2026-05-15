# Code Standards

- You must strictly follow WordPress PHP Coding standards. Pay particular attention to: security best practices (sanitization, escaping, nonces) and Yoda conditions, often missed by AI agents.
- Never add to WordPress hooks anonymous functions, it's necessary to use proper methods in each class, follow the existing code style.
- Prepend a backslash to native WordPress functions. e.g. use `\wp_create_nonce()` instead of `wp_create_nonce()`. Do not add backslashes to functions that are not native WordPress functions, like implode or is_array or similar.
- Always use static methods and properties instead of instance methods and properties.
- Order `use` import statements by line length (shortest first). This applies to all PHP files.
- Always use the null coalescing operator (`??`) where possible instead of `isset()` ternaries. For example: `$_POST['key'] ?? ''` instead of `isset( $_POST['key'] ) ? $_POST['key'] : ''`. This applies everywhere: superglobals, array access, variable fallbacks.
- Do not worry about working on code (tabs, spacing) alignment issues. We have a beautifier that will fix these automatically. Don't lose time over this.

### Security: Avoid `$_REQUEST`

Never use `$_REQUEST` in new code. Always use the explicit `$_GET` or `$_POST` superglobal that matches the expected HTTP method. Using `$_REQUEST` allows a bad actor to send different data via different HTTP verbs, which might trick a user into performing an unintended action. In some PHP configurations, cookie data is also included within `$_REQUEST`, further increasing the attack surface.

Existing `$_REQUEST` usage should be refactored when you notice them.

### Security: Avoid `filter_input_array()` without a filter definition

Never use `filter_input_array( INPUT_GET )` or `filter_input_array( INPUT_POST )` without a second argument. Without a filter definition array, it returns raw unfiltered input — it does not sanitize anything and gives a false sense of security.

When you spot this pattern, replace it: access `$_GET` or `$_POST` directly and apply proper sanitization (`sanitize_text_field()`, `absint()`, etc.) on each value where it is actually used.

### JavaScript: Use Modern Syntax

Always use `const` and `let` in new JavaScript code. Never use `var`. Use arrow functions, template literals, and other ES6+ features where appropriate. You will see outdated Javascript while working in .js files, always warn the user about it, but unless they instruct you to do otherwise, focus on modernizing the parts of the code you're working on.

### JavaScript: Variable Naming

Never abbreviate variable names. Always use full, descriptive names in camelCase. For example, use `feedbackWrapper` instead of `fw`, `reasonElement` instead of `re`, `buttonElement` instead of `btn`.

### HTML / CSS / JavaScript: Class Name Prefixing

Always prefix CSS class names with the theme/plugin prefix e.g. `debug-bar-` for Debug Bar plugin. Never use generic, unprefixed class names like `wrapper`, `actions`, `submit`. This prevents style collisions with WordPress core, other plugins, and themes.

### Code Review Standards

- Never use single-line if-return statements; always use multi-line if blocks for early returns.
- Multi-line comments must always use `/** */` blocks, never multiple `//` lines.
- Single-line comments must always use `//`.
- During code reviews, **strictly enforce** WordPress PHP Coding Standards (WPCS). Flag any violations of WPCS and explain them to the user, including but not limited to: incorrect Yoda conditions, missing sanitization/escaping, improper nonce usage, spacing issues, and naming conventions.
  WordPress PHP Coding Standards: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/php/
  WordPress Javascript Coding Standards: https://developer.wordpress.org/coding-standards/wordpress-coding-standards/javascript/

- In PHP, add a PHP doc comment block directly above every method or function, with this format:
  - A one-line summary of the method's purpose.
  - A blank line.
  - A @param tag for each param, with type and description, in this exact format:
    @param type $param_name - description.
    Add params on different lines and don't add empty lines between them.
  - A blank line.
  - A @return tag, in this exact format:
    @return type $name_if_present - description.

Maintain blank lines as shown for clarity and readability.

### Code Readability

Do not try to compress code as much as possible in as few lines as possible. Tend to favor more simple code that is extremely easy to read and understand.