# Adding a new lake

You always need at least 2 things for this:

- a UUID v4
  - UUID can be generated with `python -c 'print(__import__("uuid").uuid4())'` or by e.g. [uuidgenerator](https://www.uuidgenerator.net/version4)
- a name

Note that [scrapers][scrapers]/[tide-provider][tide-provider] PRs MUST be merged after [k8s-config][k8s-config]/[api][api] since the API needs to know about the lakes and [scrapers][scrapers] depend on [k8s-config][k8s-config].

At least one of [scrapers][scrapers] or [tide-provider][tide-provider] should be implemented for the new lake since a lake with no features doesn't make sense (it won't be displayed by any client).

## API

[repo][api]

[example PR](https://github.com/woog-life/api/pull/94)

The [api][api] simply needs to know 3 things about the lake:

- UUID (see above)
- name (see above)
- features

features are currently limited to

- temperatures ([scrapers][scrapers])
- tides ([tide-provider][tide-provider])

The example PR only contains info about `temperature`/`supportes_booking` (deprecated), the initial `tides` migration can be found here: [V8__EnabledTides.sql](https://github.com/woog-life/api/blob/main/migrations/sql/V8__EnableTides.sql)

## k8s-config

[repo][k8s-config]

[example PR](https://github.com/woog-life/k8s-config/pull/10)

[scrapers][scrapers] uses [k8s-config][k8s-config] for the lake UUIDs

## scrapers [optional]

[repo][scrapers]

[example PR](https://github.com/woog-life/scrapers/pull/34/files)

We're using scrapy here to scrape data from several different sources, check the [current implementations](https://github.com/woog-life/scrapers/tree/master/lake_scrapers/spiders) to see whether the website you want to pull data from might have been implemented already.

## tide-provider [optional]

[repo][tide-provider]

All lakes for this provider have been present from the start, no example PR yet

Currently we're using yearly data from [Federal Maritime and Hydrographic Agency of Germany (Bundesamt für Seeschifffahrt und Hydrographie)][bsh] (germany only).

This is currently executed manually once a year.

If you want to provide tidal data in a different way, please open an issue about this in the [tide-provider][tide-provider] repo.

[api]: https://github.com/woog-life/api
[k8s-config]: https://github.com/woog-life/k8s-config
[scrapers]: https://github.com/woog-life/scrapers
[tide-provider]: https://github.com/woog-life/tide-provider
[bsh]: https://www.bsh.de/DE/DATEN/Vorhersagen/Gezeiten/gezeiten_node.html
