= Theme Review: Viral Times by @hashthemes =

'''Version:''' 1.0.05  
'''Tested up to:''' 6.9  
'''Requires PHP:''' 7.2  
'''Review Date:''' 2026-05-15

----

Hi @hashthemes,

Thank you for submitting Viral Times. I have reviewed your submission against the WordPress.org theme requirements. Below are the items that need to be addressed before the theme can be approved.

----

== Critical Issues (Must Be Fixed) ==

=== 1. SQL Injection Vulnerability — attachment_url_to_postid() overrides core function ===

'''File:''' `inc/theme-functions.php`, lines 268-277

{{{
if (!function_exists('attachment_url_to_postid')) {
    function attachment_url_to_postid($attachment_src) {
        global $wpdb;
        $query = "SELECT ID FROM {$wpdb->posts} WHERE guid='$attachment_src'";
        $id = $wpdb->get_var($query);
        return $id;
    }
}
}}}

The current implementation presents two critical concerns:

 * '''SQL Injection:''' The `$attachment_src` parameter is directly interpolated into the SQL query without any escaping or prepared statement. To ensure security, please use `$wpdb->prepare()` for all database queries.
 * '''Overriding a core function:''' The theme defines `attachment_url_to_postid()` which is a WordPress core function. Themes must not override core WordPress functions. The `function_exists()` check does not make this acceptable since the core function already exists.

Please remove this function entirely and use WordPress core's `attachment_url_to_postid()` instead. Reference: https://developer.wordpress.org/reference/functions/attachment_url_to_postid/

----

=== 2. Unfiltered HTML in Author Bios — Security Risk ===

'''File:''' `inc/theme-hooks.php`, line 127

{{{
remove_filter('pre_user_description', 'wp_filter_kses');
}}}

This removes the KSES filter from user descriptions, allowing authors to inject arbitrary HTML (including potentially malicious scripts) into their bios. This is a security concern, especially on multi-author sites.

Please remove this line. If custom HTML is needed in author bios, kindly use `wp_kses_allowed_html()` to allow specific safe tags instead. Reference: https://developer.wordpress.org/reference/functions/wp_kses_allowed_html/

----

== Required Changes ==

=== 3. Missing == Description == Section in readme.txt ===

'''File:''' `readme.txt`

The readme.txt file is missing the required `== Description ==` section. Please add a detailed description of the theme to meet WordPress.org requirements.

=== 4. Missing == Frequently Asked Questions == Section in readme.txt ===

'''File:''' `readme.txt`

The readme.txt file is missing the required `== Frequently Asked Questions ==` section. Please add this section with at least one common question and answer.

=== 5. Missing == Upgrade Notice == Section in readme.txt ===

'''File:''' `readme.txt`

The readme.txt file is missing the required `== Upgrade Notice ==` section. Please add this section to inform users about important updates.

=== 6. Unescaped Output — viral_times_entry_tag() ===

'''File:''' `inc/template-tags.php`, line 400

{{{
echo $tags_list;
}}}

The `$tags_list` variable is output without proper escaping. Please ensure all data is escaped late using functions like `wp_kses_post()` right before the data is echoed. Reference: https://developer.wordpress.org/themes/theme-security/data-sanitization-escaping/

=== 7. Unescaped Output — Breadcrumb Trail ===

'''File:''' `inc/breadcrumbs.php`, line 239

{{{
echo $breadcrumb; // WPCS: XSS OK.
}}}

The `// WPCS: XSS OK.` comment is used to bypass the WordPress Coding Standards check, but the output is not properly escaped. Please ensure the breadcrumb HTML is escaped using `wp_kses_post()` or `wp_kses_allowed_html()`.

=== 8. Unescaped Output — Schema Attributes ===

'''File:''' `inc/theme-functions.php`, line 498

{{{
return apply_filters('viral_times_schema_' . $place . '_attributes', $attrs); // phpcs:ignore WordPress.Security.EscapeOutput.OutputNotEscaped
}}}

The schema attributes are returned without escaping. Please ensure the output is escaped with `esc_attr()` or similar function before being used in HTML attributes.

=== 9. Plugin Territory — Upsell in Demo Import ===

'''File:''' `inc/theme-hooks.php`, lines 244-462

The `viral_times_premium_demo_config()` function contains hardcoded upsell links to "Viral Pro" premium version with external URLs. Please ensure upselling is not obtrusive and follows WordPress.org guidelines. Consider making these links optional or less prominent.

=== 10. Unprefixed Function — breadcrumb_trail() ===

'''File:''' `inc/breadcrumbs.php`, line 35

{{{
function breadcrumb_trail($args = array()) {
}}}

The wrapper function `breadcrumb_trail()` is not prefixed with the theme slug. While this is a third-party library, the wrapper function should be prefixed with `viral_times_` to avoid conflicts with other themes or plugins.

----

== Recommendations & Notes ==

=== 11. Google Fonts — Privacy Consideration ===

'''File:''' `functions.php`, lines 276-280

The theme loads Google Fonts from Google's CDN. While the theme includes `wptt-webfont-loader.php` for local font loading, the default behavior loads fonts from Google's servers, which may have GDPR/privacy implications. I recommend enabling local font loading by default, or at least making it more prominent in the Customizer.

=== 12. Screenshot Verification ===

'''File:''' `screenshot.jpg` (214,921 bytes)

Please verify that the screenshot is exactly 1200x900px (4:3 ratio) as required by the WordPress theme review guidelines.

=== 13. extract() Usage ===

'''File:''' `inc/theme-functions.php`, line 25

{{{
function viral_times_comment($comment, $args, $depth) {
    extract($args, EXTR_SKIP);
}}}

The `extract()` function is used, which is discouraged due to potential variable collisions and security concerns. I recommend accessing `$args` directly instead.

=== 14. Missing wp_body_open() Support Check ===

'''File:''' `header.php`, line 18

{{{
<?php wp_body_open(); ?>
}}}

This is present and correct. Thank you for including this.

=== 15. Skip Link Implementation ===

'''File:''' `header.php`, line 23

{{{
<a class="skip-link screen-reader-text" href=" #ht-content"><?php esc_html_e('Skip to content', 'viral-times'); ?></a>
}}}

The skip link is present. Please note there's a space before `#ht-content` in the href attribute which should be removed for proper functionality.

----

== Summary ==

||= Category =||= Count =||
|| Critical || 2 ||
|| Required || 8 ||
|| Recommended || 5 ||

The theme has a solid foundation with good feature support (WooCommerce, Gutenberg, Customizer, RTL, translation-ready). However, there are critical security issues that must be addressed before approval, particularly the SQL injection vulnerability and the core function override. The readme.txt also needs significant improvements to meet WordPress.org requirements.

----

Thank you for your hard work on this. Once these issues are addressed, please upload the new version, and I will be happy to take another look. I look forward to your updated submission!