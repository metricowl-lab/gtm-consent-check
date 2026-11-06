# Consent Check

A web tag template for testing. It checks a page for the Consent Mode mistakes behind most cookie complaints, and reports what it finds in Preview mode. It can also push a `consent_check` event to the dataLayer.

It checks for:

1. **No consent default.** Google Consent Mode has no `default` command in the dataLayer.
2. **A default set too late.** A `gtag('config')` or `gtag('event')` command comes before the default, which usually means hard-coded Google code runs before your consent banner.
3. **Cookies without consent.** Analytics cookies are present while `analytics_storage` isn't granted, or advertising cookies while `ad_storage` isn't granted.

## Why

In Metric Owl's scans of [UK online retailers](https://www.metricowl.co.uk/article/uk-online-retailers-tracking-scan-2026) and [football club websites](https://www.metricowl.co.uk/article/football-club-websites-tracking-scan-2026), many sites kept tracking after the visitor clicked Reject all. These are the checks that would have caught it.

## Set up

1. Add the template from the gallery, or import `template.tpl`.
2. Create a tag from it and fire it on **Window Loaded**, or on your consent banner's update event.
3. In Preview, visit the site, click **Reject all**, and read the result in the browser console or in the `consent_check` event in Tag Assistant.
4. Repeat after clicking **Accept all**.

Use it while testing, or on a staging site. It isn't meant to run for every visitor.

## The result

| Field | Example |
|---|---|
| `consent_check_status` | `ok` or `issues` |
| `consent_check_issues` | `no_consent_default`, `gtag_command_before_consent_default`, `analytics_cookie_without_consent:_ga`, `ad_cookie_without_consent:_fbp`, `consent_default_unverified`, `datalayer_unreadable` |
| `consent_analytics_storage` | `granted`, `denied`, or `not_set` when there's no consent default |
| `consent_ad_storage` | `granted`, `denied`, or `not_set` when there's no consent default |

With no consent default, Tag Manager treats every consent type as granted. The check doesn't: any analytics or advertising cookie on such a page is reported.

Two results mean the template couldn't read everything:

- **`datalayer_unreadable`:** Tag Manager couldn't copy the dataLayer for the template to read, so the first two checks were skipped.
- **`consent_default_unverified`:** some dataLayer entries couldn't be read, so a default may be among them.

In both cases the cookie checks still run, using the consent state Tag Manager reports. Check the defaults in Tag Assistant's Consent tab.

## Cookies it looks for

- **Analytics:** `_ga`, which GA4 always sets.
- **Advertising:** `_gcl_au`, `_gcl_aw`, `_gcl_dc` (Google Ads), `_fbp`, `_fbc` (Meta), `_uetsid`, `_uetvid` (Microsoft), `li_fat_id` (LinkedIn), `_ttp` (TikTok), `_pin_unauth` (Pinterest), `_scid` (Snap).

A cookie set on an earlier visit, before the visitor withdrew consent, also shows up. Clear cookies before testing.

## Permissions

- **dataLayer:** reads it, and writes to it for the event.
- **Consent:** reads `analytics_storage` and `ad_storage`.
- **Cookies:** reads the cookies listed above.
- **Logging:** in Preview only.

## Licence

Apache License 2.0. See [LICENSE](LICENSE).

Built and maintained by Marc Alexander at [Metric Owl](https://www.metricowl.co.uk), a GA4 and Google Tag Manager consultancy in London. Issues and suggestions are welcome.
