# Populus Media Website Tracking Code Instructions
## Introduction
The Populus Media Website Tracking Code is designed to seamlessly integrate any website with the Populus Media Path. It serves two key functions:

1. User behavior analytics - by tracking a user's behavior on the website (e.g. pageviews, time on page, sessions, etc), we're able to tie behavior from the website to down-funnel performance within the Populus Media platform.

2. CTA connectivity - the tracking code automatically connects CTA buttons that drive to the Populus Media Path with proper user identifiers to allow cross-domain analysis.

## How to install

fm-slim is generally used by Populus Media clients to populate links in call-to-action buttons that link to a Populus Media path.

The fm-slim package picks out links from a page by looking for `fm-link` in the class of any `<a>` tags. It will replace the href inside any `<a>` tags with a link as configured in the `fm.start()` function (more on `fm.start()` below).

```html
This is my <a class="fm-link" href="">call-to-action link</a>.
```

In order to populate a link inside a call-to-action button, the fm-slim package must be included in a page. The script is generally placed inside the body of a page. It must be placed after any links that the client expects to be populated.

```html
<script
  type="text/javascript"
  src="https://fm.populus-media.net/3.2/fm-slim.js"
></script>
```

After the fm-slim package script, the `fm.start()` function must be called. For example, if the client were "zetrello," the `fm.start()` call would incorporate the client name by passing it into the `program_id` parameter.

### Google Analytics

Optionally, the client may pass a Google Analytics ID into the `fm-link` as well. **Before instantiating the `fm-slim` script** and running the `fm.start()` function, instantiate Google Analytics. Replace `UA-SOME-UA` with your Google Anayltics UA.

```html
<script type="text/javascript">
  (function(i, s, o, g, r, a, m) {
    i['GoogleAnalyticsObject'] = r;
    (i[r] =
      i[r] ||
      function() {
        (i[r].q = i[r].q || []).push(arguments);
      }),
      (i[r].l = 1 * new Date());
    (a = s.createElement(o)), (m = s.getElementsByTagName(o)[0]);
    a.async = 1;
    a.src = g;
    m.parentNode.insertBefore(a, m);
  })(
    window,
    document,
    'script',
    'https://www.google-analytics.com/analytics.js',
    'ga'
  );

  ga('create', 'UA-SOME-UA', 'auto');
</script>
```

Then, before the `fm.start()` function, append the Google Analytics client ID to the fm router:

```html
<script type="text/javascript">
  ga(function(tracker) {
    fm.router.append({ ga_id: tracker.get('clientId') });
  });

  fm.start({
    ...
  });
</script>
```

If the optional Google Analytics ID (`ga_id`) is set, the final link would look similar to the link below:

```html
This is my
<a
  class="fm-link"
  href="https://zetrello.populus-media.net/?ga_id=SOME_GA_ID&alt_source=mainsite&engagement_id=GENERATED_POPULUS_MEDIA_ENGAGEMENT_ID&master_id=GENERATED_POPULUS_MEDIA_MASTER_ID&"
  >call-to-action link</a
>.
```

### UTM

The fm script automatically picks up Urchin Tracking Module (UTM) parameters from the URL on a client's asset. Optionally, if a client would like to override or otherwise directly set UTM parameters passed to our analytics systems, UTM parameters may be added to the `fm.start()` function.

If included in the `fm.start()` function, the UTM parameters will follow a user through the path. This functionality is useful for advertising campaigns, such as those from Facebook or Google.

fm tracks the following UTM parameters:

```
  utm_source
  utm_medium
  utm_campaign
  utm_content
```

Any number of the available UTM parameters may be included in the function. For example, the `fm.start()` function below includes a `utm_content` parameter.

```html
<script type="text/javascript">
  fm.start({
    current_path: 'brand',
    program_id: 'zetrello',
    program_version: 'client',
    page_name: window.location.pathname,
    alt_source: 'mainsite',
    router: {
      linkTo: 'https://zetrello.populus-media.net/',
    },
    utm_content: 'some UTM content',
  });
</script>
```

### Alternate UTM

As with normal UTM paramaters, the fm script automatically picks up alternate UTM paramaters from the URL on a client's asset. Optionally, if a client would like to override or otherwise directly set alternate UTM parameters passed to our analytics systems, alternate UTM parameters may be added to the `fm.start()` function.

The `alt_source` parameter is used to specify the source of the link. For example, a client may use `fm` on multiple websites, and traffic coming into our path would be tracked based on source by providing a different `alt_source` value on each website.

```html
<script type="text/javascript">
  fm.start({
    current_path: 'brand',
    program_id: 'zetrello',
    program_version: 'client',
    page_name: window.location.pathname,
    alt_source: 'mainsite',
    router: {
      linkTo: 'https://zetrello.populus-media.net/',
    },
  });
</script>
```

All other UTM parameters are available as `alt_` parameters, such as `alt_medium`, etc.

With the configuration set in the `fm.start()` script above, any links in a page with a `fm-link` class will look something like the following:

```html
This is my
<a
  class="fm-link"
  href="https://zetrello.populus-media.net/?alt_source=mainsite&engagement_id=GENERATED_POPULUS_MEDIA_ENGAGEMENT_ID&master_id=GENERATED_POPULUS_MEDIA_MASTER_ID"
  >call-to-action link</a
>.
```

## Putting it all together

There are varying levels of complexity as it relates to integrating fm into a client's asset.

To get a sense of how the integration works, you may copy and paste the HTML page texts below into simple HTML files, and run them locally for testing purposes.

### Simple integration

The simplest integration, which includes only the base elements needed for an integration, would look like the following. The following would be added to any page that includes a CTA button that leads to a Populus Media path:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Zetrello</title>
  </head>
  <body>
    <a class="fm-link" href="">Path link</a>

    <script
      type="text/javascript"
      src="https://fm.populus-media.net/3.2/fm-slim.js"
    ></script>

    <script type="text/javascript">
      fm.start({
        current_path: 'brand',
        program_id: 'zetrello', // Replace [zetrello] with client name in production
        program_version: 'client',
        page_name: window.location.pathname,
        router: {
          linkTo: 'https://zetrello.populus-media.net/', // Replace [zetrello] with client name in production
        },
      });
    </script>
  </body>
</html>
```

### More complex integration

For an integration that includes Google Analytics, an explicitly-set UTM parameter and an alternate source, the page would be similar to the following:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Zetrello</title>
  </head>
  <body>
    <a class="fm-link" href="">Path link</a>

    <script type="text/javascript">
      (function(i, s, o, g, r, a, m) {
        i['GoogleAnalyticsObject'] = r;
        (i[r] =
          i[r] ||
          function() {
            (i[r].q = i[r].q || []).push(arguments);
          }),
          (i[r].l = 1 * new Date());
        (a = s.createElement(o)), (m = s.getElementsByTagName(o)[0]);
        a.async = 1;
        a.src = g;
        m.parentNode.insertBefore(a, m);
      })(
        window,
        document,
        'script',
        'https://www.google-analytics.com/analytics.js',
        'ga'
      );

      ga('create', 'UA-SOME-UA', 'auto');
    </script>

    <script
      type="text/javascript"
      src="https://fm.populus-media.net/3.2/fm-slim.js"
    ></script>

    <script type="text/javascript">
      ga(function(tracker) {
        fm.router.append({ ga_id: tracker.get('clientId') });
      });

      fm.start({
        current_path: 'brand',
        program_id: 'zetrello', // Replace [zetrello] with client name in production
        program_version: 'client',
        page_name: window.location.pathname,
        alt_source: 'mainsite',
        router: {
          linkTo: 'https://zetrello.populus-media.net/', // Replace [zetrello] with client name in production
        },
        utm_content: 'some UTM content',
      });
    </script>
  </body>
</html>
```
