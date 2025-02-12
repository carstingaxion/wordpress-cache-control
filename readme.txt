=== Cache-Control ===
Contributors: geekysoft, carstingaxion
Tags: caching, performance, cache-control, http
Requires at least: 4.4.1
Tested up to: 5.2.2
Stable tag: 2.2.5.2
License URI: https://www.gnu.org/licenses/gpl-3.0.html
License: GPLv3

Configurable HTTP Cache-Control response headers for webpages generated by WordPress.

== Description ==

I decided to fork the plugin, because there were no answers on the WordPress support forum for a long time by the original plugin author and his own statement from 2019:

>I no longer want to use a publishing platform that takes up so much of my time as WordPress does.
https://www.ctrl.blog/entry/enough-of-wordpress.html

Let's go.
---

Good caching policies is one of performance’s best friends, and it can be your new best friend too. Get friendly with intermediary and browser caches by taking control over your WordPress powered website’s HTTP `Cache-Control` headers.

This is not a caching plugin in itself, but will enable you to leverage existing standard compliant caching systems better. You can set different policies for different kinds of pages to suite your website’s needs. The Cache-Control for WordPress plugin allows you to set different policies for shared/intermediary and private caches. The plugin sets some sensible defaults for a medium traffic blog that publishes an update or two per week.

You can safely set long `Cache-Control` times as the `max-age` values is lowered automatically before a scheduled post is about to be published.

Private pages (logged in users, the admin interface, etc.) will not be cached.

The plugin has [extensive documentation](https://www.ctrl.blog/entry/wp-cache-control-documentation.html).

== Installation ==

1. Upload the plugin files to the `/wp-content/plugins/cache-control` directory, or install the plugin through the WordPress plugins screen directly.
2. Activate the plugin through the ‘Plugins’ screen in WordPress
3. Use the Settings->Cache-Control screen to configure the plugin

Optionally, also setup [mod_cache](https://httpd.apache.org/docs/current/mod/mod_cache.html) in Apache or some kind of reverse proxy like Varnish or Nginx to improve your WordPress site’s loading performance.

== Frequently Asked Questions ==

= What are Cache-Control headers and how do I use them? =

Please refer to RFC 2616 Section 13. If you’re unfamiliar with this header, you’ll want to disable this plugin until you’ve read up on caching in general. Apache’s [caching guide](https://httpd.apache.org/docs/current/caching.html) is a great resource to get better acquainted with caching. This plugin should be used with great care as it breaks assumptions set by WordPress core and most plugin authors that every page will be regenerated for every request.

= How do I test my new HTTP headers? = 

You can use your web browser’s developer tools, or a web utility for inspecting headers like [Redbot](https://redbot.org/).

= What Cache-Control headers fields can I set? =

These fields can be configured individually:

* `max-age`
* `s-maxage`
* `stale-if-error`
* `stale-while-revalidate`

Alternatively, uncacheable pages are served with `no-cache, no-store, must-revalidate`.

= Does this plugin cache and serve static content? =

No, but it tells caching reverse proxies like Apache’s mod_cache, Varnish, Squid, that it’s okay to cache the content generated by WordPress for the specified intervals you define in the header.

= What about non-public pages like the admin interface? Preview pages? Logged in users? =

These pages are always set as non-cacheable. Whether caches respect this is up to their configuration. By default, this shouldn’t be an issue.

= Can I serve stale content while revalidating asynchronously? =

Yes, the `stale-if-error` and `stale-while-revalidate` headers from RFC 5861 are supported. Please verify with your reverse proxy, content distribution network (CDN), or web accelerator whether RFC 5861 is supported there too. Setting the headers doesn’t help when your deployment doesn’t support them.

= Can I control the cacheability of Atom and RSS feeds? =

Yes, there is a separate setting for feeds. It’s recommended to set it to a few hours to preserve device battery and network bandwidth on users’ devices even if your site is updated very frequently.

= What about dynamic content? =

Caching means pages will be static (non-changing) in caches that store them. Dynamic themes that modify page content will generate one variant that is served to all users for as long as a cache consider the response “fresh”. You can use client-side scripting to make some elements (ad banners, recent posts, etc.) update more frequently than the rest of the page.

= Is this incompatible with other plugins? =

Any plugin that require dynamic content will be negatively affected. For example, many anti-comment spam plugins will not work when served statically from a cache. Make sure to test every feature on your website after enabling caching. Plugins that produce texts, images, ads, and the like will produce one output and potentially have that served to all users until the cache expires.

== Changelog ==

= 2.2.5 =

* Fix an issue an with how the plugin gets loaded in some situations.

= 2.2.4 =

* Improved security.

= 2.2.3 =

* Resolved an issue when calculating the time since the last comment.

= 2.2.1 =

* Add links to the Settings and Documentation pages from the Installed plugin page.

= 2.2.0 =

* New feature: Set `stale-if-error` and `stale-while-revalidate` fields. (RFC 5861)
* Change: A `s-maxage` value is no longer set by default.
* More extensive [plugin documentation](https://www.ctrl.blog/entry/wp-cache-control-documentation.html).

= 2.1.2 =

* Resolved a problem where sticky posts could make the front page and archives uncacheable.

= 2.1.1 =

* New feature: New filter hooks for more direct control of the Cache-Control header. See HACKING. (Thanks to Ross Featherstone.)
* Password protected posts are no longer cached after being unlocked.

= 2.0.2 =

* Resolved a compatibility issue for non-English locales. 

= 2.0.1 =

* Resolved a problem with unbuffered output during redirect handling.

= 2.0.0 =

* New feature: Enable caching of HTTP 301 Moved Permanently redirects (1 day by default).
* New feature: Stale posts (no comments and not modified) optionally multiplied by months since last activity (equal to max-age × years).
* WordPress core bug work-around: Add body message to HTTP redirects (fix for mod_cache).

= 1.2 =

* Resolved a problem that made author pages uncacheable.

= 1.1 =

* Reduce caching max-age when a scheduled post is imminent.
* Pagination factor limited to an upper value of ten times the base cache time.

= 1.0 =

* Added individual control for search result pages, 404 Not Found responses, and attachment pages.

= 0.9 =

* Initial public release.

= 0.7 =

* Here be dragons.
