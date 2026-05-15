Hi @mohammedashfaque,

Thank you for submitting the Logistic Transport theme. I have reviewed your submission against the WordPress.org theme requirements. Below are the items that need to be addressed before the theme can be approved.

## Required Changes

### Security & Escaping

**Unescaped Output in Custom Front Page**
- **File:** `page-template/custom-frontpage.php`, line 138
- **Issue:** The `$video_html` variable is output directly without escaping: `echo $video_html;`
- **Fix:** Please escape this output using `wp_kses()` or `esc_html()`. Since this contains embedded media, consider using `wp_kses()` with appropriate allowed HTML tags or `wp_kses_post()`.

### Theme Guidelines

**Demo Import Functionality**
- **File:** `inc/dashboard/demo-importer.php` (referenced in `getstart.php`)
- **Issue:** Themes must not include demo import functionality. Demo imports are plugin territory.
- **Fix:** Please remove the demo import feature from the theme. You can recommend a plugin for this functionality instead.

### Code Quality

**Malformed HTML**
- **File:** `footer.php`, lines 122-123
- **Issue:** There are double closing `</footer>` tags.
- **Fix:** Remove the duplicate closing tag on line 123.

**Missing Translation Text Domain**
- **File:** `inc/customizer.php` (multiple locations)
- **Issue:** Customizer labels are hardcoded without translation functions. Examples:
  - Line 253: `'label' => 'Theme Color Option 1'`
  - Line 263: `'label' => 'Theme Color Option 2'`
  - Line 925: `'label' => 'Preloader Color'`
  - Line 935: `'label' => 'Preloader Background Color'`
- **Fix:** Wrap all strings in `__()` with the text domain: `__( 'Theme Color Option 1', 'logistic-transport' )`

**PHP Syntax Error**
- **File:** `page-template/custom-frontpage.php`, line 49
- **Issue:** The condition `if ( get_theme_mod('logistic_transport_slider_button_text','Read More') != '' && get_theme_mod('logistic_transport_slider_button',true) != '' || get_theme_mod('logistic_transport_slider_button_responsive',true) != ''|| get_theme_mod('logistic_transport_slider_button_link') != '')` is missing a closing parenthesis before `{`.
- **Fix:** Add the missing closing parenthesis.

**Unused Variable**
- **File:** `functions.php`, line 415
- **Issue:** `$contents = logistic_transport_wptt_get_webfont_url( esc_url_raw( $fonts_url ) );` is assigned but never used.
- **Fix:** Remove this unused variable assignment.

**Incorrect Escaping for CSS Values**
- **File:** `functions.php`, lines 494-548
- **Issue:** Using `esc_html()` for CSS values in inline styles. While functional, `esc_attr()` is more semantically correct for CSS attribute values.
- **Fix:** Change `esc_html()` to `esc_attr()` for all CSS values in the inline style generation.

**Commented Code**
- **File:** `functions.php`, lines 162-163
- **Issue:** Leftover commented code: `// &&` and `// isset( $_GET['activated'] )`
- **Fix:** Remove the commented code if no longer needed.

## Recommendations

### Code Organization
- Consider removing `var_dump()` or debug statements if present in any development files before submission.
- The `wp_enqueue_style( 'logistic-transport-block-patterns-style-frontend'...` in `functions.php` line 421 references a file that may not exist in all contexts. Please verify this file exists or add proper error handling.

### Best Practices
- The theme uses `get_template_directory_uri()` extensively. Consider caching these values in a variable if used multiple times in the same function to improve performance.
- The inline CSS generation in `functions.php` could benefit from better organization, possibly using a dedicated class or separate file for maintainability.

## Closing

Thank you for your hard work on this theme. The theme has a solid foundation with good use of WordPress APIs, proper theme support declarations, and comprehensive customizer options. Once these issues are addressed, please upload the new version, and I will be happy to take another look. I look forward to your updated submission!

---
**Reviewer Notes:** Theme version 1.6.4 reviewed against WordPress.org Theme Directory requirements.