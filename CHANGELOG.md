## [Unreleased] - 2025-09-20

### Changed
- Renamed the internal `prelude` module (formerly `predule`) and aligned all imports to match the conventional name.
- `addr::accessor::create_http_client_by_ctrl` now returns `AddrResult<reqwest::Client>`, allowing callers such as `HttpAccessor` to surface timeout/proxy configuration errors instead of falling back silently.
- Refined `GitRepository`'s builder-style API: consolidated optional setters, clarified the serialized `resource` field, and de-duplicated credential loading through a shared helper.
- Updated the access control tests to call `save_yaml` instead of the deprecated `save_yml` helper so clippy passes with `-D warnings`.
