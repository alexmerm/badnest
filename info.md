# badnest

A bad Nest thermostats integration that uses the web api to work after Works with Nest was shut down (bad Google, go sit in your corner and think about what you did)

## Why is it bad?

This isn't an advertised or public API, it's still better than web scraping, but will never be as good as the original API

## Drawbacks

- No proper error handling
- Won't work with 2FA enabled accounts
- Will only work the for thermostat, I have no other devices to test with
- Nest could change their webapp api at any time, making this defunct
- Presets don't work (Eco, Away)

## Example configuration.yaml - When you're not using the Google Auth Login

```yaml
badnest:
  login_type: nest
  email: email@domain.com
  password: !secret nest_password

climate:
  - platform: badnest
    scan_interval: 10
```
## Example configuration.yaml - When you are using the Google Auth Login
```yaml
badnest:
  login_type: google
  issue_token: "https://accounts.google.com/o/oauth2/iframerpc....."
  cookie: "OCAK=......"
  api_key : "#YOURAPIKEYHERE#"

climate:
  - platform: badnest
    scan_interval: 10
```

With many thanks to: chrisjshull from https://github.com/chrisjshull/homebridge-nest/

The values of `"issue_token"`, `"cookie"` and `"api_key"` are specific to your Google Account. To get them, follow these steps (only needs to be done once, as long as you stay logged into your Google Account).

1. Open a Chrome browser tab in Incognito Mode (or clear your cache).
2. Open Developer Tools (View/Developer/Developer Tools).
3. Click on 'Network' tab. Make sure 'Preserve Log' is checked.
4. In the 'Filter' box, enter `issueToken`
5. Go to `home.nest.com`, and click 'Sign in with Google'. Log into your account.
6. One network call (beginning with `iframerpc`) will appear in the Dev Tools window. Click on it.
7. In the Headers tab, under General, copy the entire `Request URL` (beginning with `https://accounts.google.com`, ending with `nest.com`). This is your `"issue_token"` in `configuration.yaml`.
8. In the 'Filter' box, enter `oauth2/iframe`
9. Several network calls will appear in the Dev Tools window. Click on the last `iframe` call.
10. In the Headers tab, under Request Headers, copy the entire `cookie` (beginning `OCAK=...` - **include the whole string which is several lines long and has many field/value pairs** - do not include the `cookie:` name). This is your `"cookie"` in `configuration.yaml`.
11. In the 'Filter' box, enter `issue_jwt`
12. Click on the last `issue_jwt` call.
13. In the Headers tab, under Request Headers, copy the entire `x-goog-api-key` (do not include the `x-goog-api-key:` name). This is your `"api_key"` in `configuration.yaml`.

