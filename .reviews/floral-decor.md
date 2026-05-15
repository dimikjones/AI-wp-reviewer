# Theme Review: Floral Decor by @abuturab

**Version:** 1.0.0  
**Tested up to:** 6.9  
**Requires PHP:** 7.2 (style.css) / 5.6 (readme.txt)  
**Review Date:** 2026-05-15

---

## Required Changes

### 1. Missing `add_theme_support( 'title-tag' )`

**File:** `functions.php`, line 42

The theme does not declare support for the title tag. This is a required theme feature per WordPress.org guidelines.

**Fix:** Add `add_theme_support( 'title-tag' );` to the `floral_decor_setup()` function.

---

### 2. Missing `== Frequently Asked Questions ==` Section in readme.txt

**File:** `readme.txt`

The readme.txt file is missing the required `== Frequently Asked Questions ==` section.

---

### 3. Missing `== Upgrade Notice ==` Section in readme.txt

**File:** `readme.txt`

The readme.txt file is missing the required `== Upgrade Notice ==` section.

---

### 4. PHP Version Mismatch Between Files

**Files:** `style.css` and `readme.txt`

- style.css: `Requires PHP: 7.2`
- readme.txt: `Requires PHP: 5.6`

These version requirements must match. Since the theme is a block theme using modern WordPress features, please update readme.txt to require PHP 7.2 or later.

---

### 5. Duplicate templateParts Entry in theme.json

**File:** `theme.json`, lines 578-587

The "sidebar" template part is registered twice in the templateParts array. Please remove the duplicate entry.

```json
{
    "area": "uncategorized",
    "name": "sidebar",
    "title": "Sidebar"
},
{
    "area": "uncategorized",
    "name": "sidebar",
    "title": "Sidebar"
}
```

---

## Recommended Changes

### 6. Hardcoded Contact Information

**File:** `patterns/header-default.php`, lines 13, 17

Email (`flowershop12@example.com`) and phone (`+(00) 123 456 789`) are hardcoded in the header pattern. Consider making these customizable via theme options or use more obvious placeholder text.

---

### 7. Empty Alt Attributes on Images

**Files:** `patterns/slider-section.php`, `patterns/front-page.php`

Multiple images have empty alt attributes (`alt=""`). While this may be intentional for decorative images, ensure they are properly marked as decorative or provide meaningful alternative text for accessibility.

---

### 8. Excessive CSS !important Usage

**File:** `style.css`

The stylesheet makes heavy use of `!important` (e.g., lines 60-61, 90-94). This makes customization difficult for users. Consider reducing `!important` usage where specificity can be achieved through proper CSS cascade.

---

### 9. 404 Page Letter Spacing

**File:** `patterns/hidden-404.php`, line 14

The "404" heading uses `letter-spacing: 50px` which may cause readability issues. Consider reducing this value for better accessibility.

---

## Security & Code Quality

### 10. Proper Escaping and Sanitization

✅ **Good:** The theme properly escapes output using `esc_url()`, `esc_html_e()`, and `esc_attr()` throughout pattern files.

### 11. Text Domain Usage

✅ **Good:** All text strings use the correct text domain `floral-decor` and are properly internationalized.

### 12. Prefixing

✅ **Good:** All functions, hooks, and CSS classes are properly prefixed with `floral-decor` or `floral_decor`.

---

## Positive Observations

- **Block Theme Structure:** Well-organized FSE theme with proper templates, parts, and patterns
- **Local Fonts:** Excellent use of locally-hosted fonts (Jost, Italiana, Josefin Sans) for GDPR compliance
- **WooCommerce Support:** Properly declared WooCommerce support with dedicated templates
- **RTL Support:** RTL stylesheet included
- **Translation Ready:** `.pot` file present in languages directory
- **Accessibility:** Skip links are handled automatically by block theme `<main>` element
- **Code Organization:** Clean file structure with logical separation of concerns

---

## Summary

| Category | Count |
|----------|-------|
| Required | 5 |
| Recommended | 5 |
| Positive | 7 |

**Overall Assessment:** Floral Decor is a well-built block theme with good localization practices and modern WordPress standards. The required changes are minor documentation and configuration issues that should be quick to resolve. The theme demonstrates solid understanding of block theme architecture and performs well on security and internationalization requirements.

Once the required items are addressed, this theme should be ready for approval.