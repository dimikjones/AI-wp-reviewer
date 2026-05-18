= Theme Review: Viral Times by @hashthemes =

'''Version:''' 1.0.05  
'''Tested up to:''' 6.9  
'''Requires PHP:''' 7.2  
'''Review Date:''' 2026-05-15

----

== Critical Issues ==

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

'''Problems:'''
 * '''SQL Injection:''' The `$attachment_src` parameter is directly interpolated into the SQL query without any escaping or prepared statement. This is a critical security vulnerability.
 * '''Overriding a core function:''' The theme defines `attachment_url_to_postid()` which is a WordPress core function. This is not allowed — themes must not override core WordPress functions. The `function_exists()` check does not make this acceptable since the core function already exists.

'''Fix:''' Remove this function entirely. Use WordPress core's `attachment_url_to_postid()` instead.

----

=== 2. Unfiltered HTML in Author Bios — Security Risk ===

'''File:''' `inc/theme-hooks.php`, line 127

{{{
remove_filter('pre_user_description', 'wp_filter_kses');
}}}

This removes the KSES filter from user descriptions, allowing authors to inject arbitrary HTML (including potentially malicious scripts) into their bios. This is a security concern, especially on multi-author sites.

'''Fix:''' Remove this line. If custom HTML is needed in author bios, use `wp_kses_allowed_html()` to allow specific safe tags instead.

----

== Required Changes ==

=== 3. Missing == Description == Section in readme.txt ===

'''File:''' `readme.txt`

The readme.txt file is missing the required `== Description ==` section. This section must provide a detailed description of the theme.

=== 4. Missing == Frequently Asked Questions == Section in readme.txt ===

'''File:''' `readme.txt`

The readme.txt file is missing the required `== Frequently Asked Questions ==` section.

=== 5. Missing == Upgrade Notice == Section in readme.txt ===

'''File:''' `readme.txt`

The readme.txt file is missing the required `== Upgrade Notice ==` section.

=== 6. Unescaped Output — viral_times_entry_tag() ===

'''File:''' `inc/template-tags.php`, line 400

{{{
echo $tags_list;
}}}

The `$tags_list` variable (returned by `get_the_tag_list()`) is output without escaping. While `get_the_tag_list()` returns HTML, it should be escaped with `wp_kses_post()`.

=== 7. Unescaped Output — Breadcrumb Trail ===

'''File:''' `inc/breadcrumbs.php`, line 239

{{{
echo $breadcrumb; // WPCS: XSS OK.
}}}

The `// WPCS: XSS OK.` comment is used to bypass the WordPress Coding Standards check, but the output is not properly escaped. The breadcrumb HTML should be escaped or the `wp_kses_allowed_html()` should be used.

=== 8. Unescaped Output — Schema Attributes ===

'''File:''' `inc/theme-functions.php`, line 498

{{{
return apply_filters('viral_times_schema_' . $place . '_attributes', $attrs); // phpcs:ignore WordPress.Security.EscapeOutput.OutputNotEscaped
}}}

The schema attributes are returned without escaping. While this is used in HTML attributes context, the output should be escaped with `esc_attr()` or similar.

=== 9. Plugin Territory — Upsell in Demo Import ===

'''File:''' `inc/theme-hooks.php`, lines 244-462

The `viral_times_premium_demo_config()` function contains hardcoded upsell links to "Viral Pro" premium version with external URLs (`https://hashthemes.com/wordpress-theme/viral-pro/`). This is considered obtrusive upselling and should be toned down or removed.

=== 10. Unprefixed Function — breadcrumb_trail() ===

'''File:''' `inc/breadcrumbs.php`, line 35

{{{
function breadcrumb_trail($args = array()) {
}}}

The wrapper function `breadcrumb_trail()` is not prefixed with the theme slug. While this is a third-party library (Breadcrumb Trail by Justin Tadlock), the wrapper function should be prefixed to avoid conflicts.

----

== Recommended Changes ==

=== 11. Google Fonts — Privacy Consideration ===

'''File:''' `functions.php`, lines 276-280

The theme loads Google Fonts from Google's CDN. The theme includes `wptt-webfont-loader.php` for local font loading (line 326), which is good. However, the default behavior loads fonts from Google's servers, which may have GDPR/privacy implications.

'''Recommendation:''' Enable local font loading by default, or at least make it more prominent in the Customizer.

=== 12. Screenshot Verification ===

'''File:''' `screenshot.jpg` (214,921 bytes)

The screenshot file exists. Verify that it is exactly 1200x900px (4:3 ratio) as required by the WordPress theme review guidelines.

=== 13. extract() Usage ===

'''File:''' `inc/theme-functions.php`, line 25

{{{
function viral_times_comment($comment, $args, $depth) {
    extract($args, EXTR_SKIP);
}}}

The `extract()` function is used, which is discouraged due to potential variable collisions and security concerns. Consider accessing `$args` directly.

=== 14. Missing wp_body_open() Support Check ===

'''File:''' `header.php`, line 18

{{{
<?php wp_body_open(); ?>
}}}

This is present and correct. Good.

=== 15. Skip Link Implementation ===

'''File:''' `header.php`, line 23

{{{
<a class="skip-link screen-reader-text" href=" #ht-content"><?php esc_html_e('Skip to content', 'viral-times'); ?></a>
}}}

The skip link is present. Note there's a space before `#ht-content` in the href attribute which should be removed.

----

== Summary ==

||= Category =||= Count =||
|| Critical || 2 ||
|| Required || 8 ||
|| Recommended || 5 ||

'''Overall Assessment:''' The theme has a solid foundation with good feature support (WooCommerce, Gutenberg, Customizer, RTL, translation-ready). However, there are critical security issues that must be addressed before approval, particularly the SQL injection vulnerability and the core function override. The readme.txt also needs significant improvements to meet WordPress.org requirements.