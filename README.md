# Qaiyo Smart Appointment

> WordPress appointment booking built on a fluid slot engine: instead of slicing the day into one fixed grid, it finds the real free gaps around existing bookings and fits each service into them — a 2-hour and a 30-minute service share the same afternoon without wasting a minute. Unlimited services, staff and bookings in the free plugin; payments, extra booking modes and a customer portal in the Pro add-on.

[![WordPress 6.2+](https://img.shields.io/badge/WordPress-6.2%2B-21759b.svg)](https://wordpress.org/)
[![PHP 7.4+](https://img.shields.io/badge/PHP-7.4%2B-777bb4.svg)](https://www.php.net/)
[![License: GPL v2+](https://img.shields.io/badge/License-GPLv2%2B-blue.svg)](https://www.gnu.org/licenses/gpl-2.0)
[![Free](https://img.shields.io/badge/free-1.3.0-6c5ce7.svg)](#free-plugin)
[![Pro](https://img.shields.io/badge/pro-1.3.0-f39c12.svg)](#pro-add-on)

This repository hosts the **free** Qaiyo Smart Appointment plugin — live on
[WordPress.org](https://wordpress.org/plugins/qaiyo-smart-appointment/). The optional
[Qaiyo Smart Appointment Pro](https://qaiyo-plugins.com/qaiyo-smart-appointment/) add-on adds Stripe
and PayPal without a shop, deposits, five more booking modes, a customer booking portal, a waiting
list, locations, staff days off and custom branding.

- **Website:** [qaiyo-plugins.com](https://qaiyo-plugins.com)
- **Support:** info@qaiyo-plugins.com
- **Issues:** [GitHub Issues](../../issues)

---

## Why this plugin

Most booking plugins decide the day's slots before anyone books: one duration, one grid, 9:00,
9:30, 10:00. Book a 45-minute service at 9:00 and the 9:30 and 10:00 slots are both gone — the
calendar fills up with gaps nobody can use.

Qaiyo Smart Appointment works the other way round. **Availability is computed from the bookings
that exist**: the engine takes the working hours, subtracts every stored block (buffers included),
and offers each service at every step-grid start where it still fits. The remaining time stays
bookable for any mix of longer and shorter services.

The plugin is designed to be:

- **Correct under load** — a booking is re-checked the moment it is stored, and every status
  change is compare-and-set, so two customers clicking the same slot at the same instant can never
  both keep it, and a double-clicked "Approve" acts once.
- **Honest to the customer** — the time a customer sees is the time the service runs; buffers are
  reserved silently around it.
- **Private by default** — the public calendar and the "Upcoming" card show what runs and when,
  never who booked. Bookings are part of WordPress's own personal-data export and erase tools.
- **Safe against scanners and bots** — an emailed link only ever opens a confirmation screen; the
  change is a separate POST, so a mail scanner fetching every URL changes nothing. The booking form
  carries a spam trap and a per-connection limit.
- **Extensible, never coupled** — the Pro add-on registers its modes, providers, fields and emails
  through the same documented filters your own code can use. The free plugin never checks a
  licence and never gates a free feature.
- **Translation-ready** — ten languages besides English, Polylang / WPML / TranslatePress
  compatible.

---

## Free plugin

### Booking

| Feature | What it does |
|---|---|
| **Fluid slot engine** | Starts on a configurable step grid, inside the real free gaps. Mixed durations share a day without dead time. |
| **Fixed mode** | Classic back-to-back bands, site-wide or per service. |
| **Quote requests** | A mode that reserves nothing: the customer names a day and a part of the day and asks for a price. Enquiries never block a booking and never collide. |
| **Buffers** | Time before and after a service, reserved but never shown as part of the booking. |
| **Staff** | "Anyone available" or a specific person; per-person working hours with split shifts, falling back to the site-wide hours. |
| **Soft hold** | A pending request can leave the time open until approved; the first approval wins, and a conflicting approval is refused. |
| **Days off** | Site-wide closed days and holidays. |
| **Payments** | A service can require payment before it is confirmed. WooCommerce is the built-in provider; an unpaid order releases its slot through the store's own "hold stock" timeout. |

### On the page

| Placement | Shortcode | Also available as |
|---|---|---|
| Booking form | `[qsab_booking service="" layout=""]` | Gutenberg block, Elementor, Bricks, Breakdance, Oxygen |
| Availability calendar | `[qsab_availability service="" staff="" days="7"]` | Gutenberg block, Elementor, Bricks, Breakdance, Oxygen |
| "Upcoming" card | `[qsab_upcoming title="" count="3" service="" staff="" show_date="1" theme="auto" audience="everyone"]` | Gutenberg block, Elementor, Bricks, Breakdance, Oxygen |

Every placement renders through one server-side renderer, so a builder widget and a shortcode
produce identical markup. The builder controls come from one shared definition
(`Qsab_Widgets`), so a setting cannot drift between builders.

**The "Upcoming" card** is an agenda for a reception screen, a team page or a "what's on" box:
today's date and booking count on a tile, then the next confirmed bookings with their service
colour and time. It contains no customer name, email, phone or note — only what runs and when —
so it is safe on a public page. `theme="auto"` follows the visitor's light / dark setting;
`audience="logged_in"` keeps it to logged-in users.

### Emails

Six messages — request received, confirmed, cancelled, completed, reminder, and the admin alert —
each with its own switch, subject and wording, written with merge tags (`{customer_name}`,
`{service}`, `{when}`, `{price}`, `{cancel_link}`, …). The admin alert carries signed one-click
Approve / Decline buttons. Reminders go out from an hourly sweep that claims each booking before
mailing it, so overlapping cron runs never send one twice.

### Admin

Dashboard, appointments list (paged, filterable, bulk delete), services, staff, working hours,
per-form settings saves, settings export / import, a data-on-uninstall switch, and FluentCRM sync
(an existing contact keeps its subscription status).

---

## Pro add-on

The Pro add-on is a separate plugin that requires the free one. Everything above keeps working
without it.

| Module | What it adds |
|---|---|
| **Stripe & PayPal** | Hosted payment pages, no shop needed. A booking confirms from the signed webhook, the customer's return (looked up at the provider) or an hourly check; an abandoned payment releases its slot after two hours. |
| **Deposits** | A percentage or fixed amount per service, with any provider. |
| **Booking modes** | Group sessions, hourly and multi-day rentals (the customer picks the length), an arrival-order queue, and recurring series — charged once, with the later dates following the first. |
| **Customer portal** | `[qsabp_my_bookings]`: a 30-minute emailed link to every upcoming booking, with cancel buttons. It never reveals whether an address has bookings. |
| **Waiting list** | Customers join a full day; each freed slot emails the next person in line. |
| **Locations** | Each with its own opening hours; a selector on the form and a `{location}` email tag. |
| **Staff days off** | Date-specific absences per person. |
| **Availability** | A daily break per weekday and per-service booking windows. |
| **Branding & layouts** | Accent colour, font, logo, and three more booking-form layouts. |

---

## Installation

### From a ZIP file

1. Install from **Plugins → Add New** (search "Qaiyo Smart Appointment"), or download a
   [release ZIP](../../releases) and use **Plugins → Add New → Upload Plugin**.
2. Activate. A **Smart Booking** menu appears.
3. Add services under **Smart Booking → Services**, set your hours under **Settings**, and place
   `[qsab_booking]` (or the block / widget) on a page.

For the Pro add-on: activate the free plugin first (Pro declares it as a required plugin and
refuses to run without it), upload `qaiyo-smart-appointment-booking-pro.zip`, and enter the
licence key under **Smart Booking → License**.

### From source (developers)

```bash
git clone https://github.com/qaiyo/qaiyo-smart-appointment.git
ln -s "$(pwd)/qaiyo-smart-appointment" /path/to/wordpress/wp-content/plugins/qaiyo-smart-appointment
```

**Requirements:** WordPress 6.2+ (tested up to 7.1), PHP 7.4+.
Optional: WooCommerce for the built-in payment provider, FluentCRM for contact sync, Qaiyo Admin
Booster for the dashboard card.

---

## Developer API

The Pro add-on uses **only** these hooks — an add-on of your own can do exactly the same.

### Filters — booking engine

| Filter | Purpose |
|---|---|
| `qsab_booking_modes` | Register a booking mode (`key => label`). |
| `qsab_mode_slots` | Generate the slots of your own mode; return `null` to fall through to the fluid engine. |
| `qsab_available_slots` | The final slot list of a service and date. |
| `qsab_staff_is_available` | Take a person out on a date. |
| `qsab_blocking_statuses` | Which statuses occupy time (soft hold switches pending off). |
| `qsab_non_blocking_services` | Services whose bookings reserve nothing. |
| `qsab_booking_slot` | Adjust or refuse a recomputed slot just before it is booked (return a `WP_Error`). |
| `qsab_slot_payload` | Extra keys sent to the booking form for one slot. |
| `qsab_no_slots_available` | Attach something (a waiting-list offer) to an empty day. |
| `qsab_booking_rate_limit` | Bookings per client per hour (default 10, `0` = off). |
| `qsab_client_ip` | Resolve the client address behind a known proxy. Only `REMOTE_ADDR` is trusted by default. |

### Filters — payments

| Filter | Purpose |
|---|---|
| `qsab_payment_providers` | Register a provider once it can actually take money. |
| `qsab_payment_url` | The hosted payment page of a booking. |
| `qsab_payment_required` | Exempt one booking (a later date of a paid-up series). |
| `qsab_payment_amount` / `qsab_payment_label` | The amount charged and the line-item label. |
| `qsab_format_price` / `qsab_currency_symbol` / `qsab_currency_position` | Price display. |

### Filters — emails, output and admin

| Filter | Purpose |
|---|---|
| `qsab_email_types` | Register a notification of your own (it gets the switch and the editor). |
| `qsab_email_enabled` / `qsab_email_subject` / `qsab_email_body` / `qsab_email_html` | Per-message control. |
| `qsab_email_tags` / `qsab_email_documented_tags` | Add a merge tag and list it in the editor. |
| `qsab_fluentcrm_contact_status` | Status of a new FluentCRM contact (`subscribed`, `pending`, `transactional`). |
| `qsab_branding` | Accent, font, radius and logo tokens — validated for the CSS context on output. |
| `qsab_layouts` / `qsab_booking_layout` | Register a booking-form layout; choose one. |
| `qsab_widget_fields` | Add a control to every page-builder widget at once. |
| `qsab_widget_allowed_html` | Tags an add-on injects into widget output. |
| `qsab_service_save_data` | Save your own per-service fields. |
| `qsab_ecosystem_card_body` | Replace the dashboard card's body. |
| `qsab_pro_active` / `qsab_pro_unlocked_features` / `qsab_upgrade_url` | The locked-feature grid. |
| `shortcode_atts_qsab_booking` / `shortcode_atts_qsab_upcoming` / `shortcode_atts_qsab_availability` | Standard WordPress shortcode attribute filters. |

### Actions

| Action | Fires |
|---|---|
| `qsab_appointment_created` | After a booking survived its re-check (`$id, $row`). |
| `qsab_appointment_status_changed` | Once per real status change (`$id, $status`). |
| `qsab_appointment_deleted` | After a row is gone — drop what you stored against it. |
| `qsab_booking_form_top` | Inside the booking form, above the steps. |
| `qsab_service_form_fields` | Inside the service editor. |
| `qsab_after_tabs` | After the admin tab bar. |

**Pro side:** `qsabp_payment_ttl` (how long an unpaid booking holds its slot, 30 min – 24 h),
`qsabp_my_bookings_email`, `qsabp_waitlist_booking_url`.

The booking form also dispatches DOM events — `qsab:slots-rendered`, `qsab:slot-selected`,
`qsab:summary`, `qsab:collect` — so an add-on can extend it without replacing markup.

---

## Translations

Ten languages ship in `/languages/` next to the `.pot`:

```
qaiyo-smart-appointment-hu_HU    qaiyo-smart-appointment-it_IT
qaiyo-smart-appointment-de_DE    qaiyo-smart-appointment-ru_RU
qaiyo-smart-appointment-fr_FR    qaiyo-smart-appointment-tr_TR
qaiyo-smart-appointment-es_ES    qaiyo-smart-appointment-pl_PL
qaiyo-smart-appointment-ja       qaiyo-smart-appointment-pt_PT
```

The `.pot` is generated from the code with `wp i18n make-pot`, so it can never list a string the
plugin does not use; plural forms (`_n()`) and contexts (`_x()`) are carried through to every
locale. The block editor script only uses WordPress's own "Settings" string — every label it shows
comes from PHP — so no separate JSON catalogue is needed. Locale variants fall back to their base
locale (`de_AT → de_DE`, `ja_JP → ja`).

The WordPress.org package deliberately contains **no** `.po`/`.mo`: translate.wordpress.org
serves those once the plugin is listed. The self-hosted "full" ZIP bundles them.

---

## Standards & security

- Classes `Qsab_*` / `Qsabp_*`, and every stored name — tables, options, transients, meta, script
  handles, shortcodes — carries the `qsab` / `qsabp` prefix.
- Every admin action is capability- **and** nonce-checked; every public endpoint verifies its
  nonce or signed key first. `$_POST`/`$_GET` values are unslashed and validated field by field.
- Emailed links are HMAC-signed per booking and per action, compared in constant time, and only
  ever open a confirmation screen; the change is a separate POST. Those screens send
  `Referrer-Policy: no-referrer` and are never cached or indexed.
- Every query is prepared, table names included (`%i`, hence WordPress 6.2+). Dynamic `IN ()`
  lists are built from generated placeholders only.
- Everything printed is escaped where it is printed; builder output passes a `wp_kses` allowlist,
  and CSS values are validated for the CSS context rather than HTML-escaped. The escaping sniff is
  run with `--ignore-annotations`.
- Stripe webhooks are verified (signature + timestamp tolerance) before anything in them is read;
  a return URL is never believed — the payment is looked up at the provider.
- Rate limits key on `REMOTE_ADDR` only and store a salted hash, never the address.
- Multisite-aware uninstall that keeps data unless it has been explicitly armed.

---

## Development

### Repository layout

```
qaiyo-smart-appointment.php         Main plugin file (header, constants, loading, DB upgrade)
uninstall.php                       Multisite-aware cleanup, only when armed
includes/
  class-qsab-cast.php               Checked narrowing for untyped values
  class-qsab-settings.php           Settings shape, defaults, per-section saves
  class-qsab-install.php            Schema (dbDelta) + first-install seed
  class-qsab-services.php           Services repository
  class-qsab-staff.php              Staff repository + service assignments
  class-qsab-schedule.php           Working hours
  class-qsab-appointments.php       Appointments repository, status CAS, approvals
  class-qsab-occupancy.php          "Is this time free?" — the single answer, race re-check
  class-qsab-slots.php              The fluid / fixed / quote slot engine + add-on toolkit
  class-qsab-booking-ajax.php       The two public AJAX endpoints
  class-qsab-public-links.php       Cancel / approve / decline confirmation screens
  class-qsab-manage-link.php        HMAC keys for the emailed links
  class-qsab-rate-limiter.php       Fixed-window limiter on transients
  class-qsab-frontend.php           [qsab_booking], assets, branding CSS
  class-qsab-availability.php       [qsab_availability]
  class-qsab-upcoming.php           [qsab_upcoming]
  class-qsab-widgets.php            Shared widget definitions for every builder
  class-qsab-emails.php             Email catalogue, overrides, merge tags
  class-qsab-notifications.php      Who gets which email when
  class-qsab-reminders.php          Hourly reminder sweep
  class-qsab-payments.php           Provider registry
  class-qsab-woocommerce.php        Built-in WooCommerce provider
  class-qsab-fluentcrm.php          FluentCRM sync
  class-qsab-privacy.php            Personal-data exporter / eraser
  class-qsab-kses.php, -css.php     Output allowlists and CSS validation
  class-qsab-view.php               Template renderer (traversal-guarded)
  class-qsab-admin*.php             Admin menu, actions, notices
  builders/                         Gutenberg, Elementor, Bricks, Breakdance, Oxygen adapters
admin/                              Admin screen templates
templates/                          Front-end templates (booking form, "Upcoming" card)
assets/                             CSS + vanilla JS
languages/                          Ten translations + the .pot
```

Everything in this folder is what runs on a site — it is copied verbatim to the WordPress.org SVN.
Tests, static analysis, build scripts and the translation generator live in the sibling
`qaiyo-smart-appointment-dev-tools/` folder, which also covers the Pro add-on:

```
../qaiyo-smart-appointment-dev-tools/
  composer.json                     PHPUnit 9 + Brain Monkey + PHPStan (dev only)
  phpunit.xml.dist                  Free + Pro suites, coverage scope
  phpstan.neon.dist                 Level 10, both plugins
  tests/                            Unit tests + an in-memory SQLite $wpdb (Support/)
  build-scripts/                    Release ZIPs + Plugin Check runner
  languages/                        Translation tables + the .pot/.po/.mo builder
  README.md                         Internal notes: data model, contract, exceptions
```

### Quality gates

Run from `../qaiyo-smart-appointment-dev-tools/` before every release:

```bash
composer install
composer test           # PHPUnit 9 + Brain Monkey — 242 tests (170 free / 72 Pro)
composer analyse        # PHPStan level 10 — must be zero errors, no ignoreErrors
composer coverage       # line + branch coverage via Xdebug
composer i18n           # regenerate .pot/.po/.mo — fails while any string is untranslated
composer build          # release ZIPs
composer plugin-check   # Plugin Check on the unpacked packages
```

From the workspace root:

```bash
phpcs --standard=WordPress --sniffs=WordPress.Security.EscapeOutput --ignore-annotations qaiyo-smart-appointment
for f in qaiyo-smart-appointment/languages/*.po; do msgfmt --check-format -o /dev/null "$f"; done
```

### Building a release ZIP

```bash
cd ../qaiyo-smart-appointment-dev-tools
composer build   # → ../qaiyo-smart-appointment.zip        (WordPress.org, no .po/.mo)
                 # → ../qaiyo-smart-appointment-full.zip   (self-hosted, with translations)
                 # → ../qaiyo-smart-appointment-booking-pro.zip
```

For WordPress.org itself the plugin folder is copied to SVN directly — without
`languages/*.po` and `languages/*.mo`, exactly like the WordPress.org ZIP.

---

## Contributing

Bug reports and pull requests are welcome via [GitHub Issues](../../issues).

Please follow the existing style (tabs, WPCS, PHPDoc on public methods), add tests for new logic
under `qaiyo-smart-appointment-dev-tools/tests/Unit/`, and add a changelog entry to `readme.txt`.
Two rules worth knowing first: a **filter callback never gets a parameter or return type** — core
and other plugins pass anything through filters, and a typed callback turns that into a fatal
error (a test enforces it); and any check-then-write on a booking must be **compare-and-set**, never
a plain update after a read.

---

## License

GPL-2.0-or-later. See <https://www.gnu.org/licenses/gpl-2.0.html>.

---

## Credits

Made by **[Qaiyo](https://qaiyo-plugins.com)**.
Part of the Qaiyo plugin family — a set of WordPress plugins that share a brand, a design system,
and a coordinated admin experience.

Contact: info@qaiyo-plugins.com
