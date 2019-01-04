# Firefly XD Website Tracking Code Instructions
## Introduction
The Firefly XD Website Tracking Code is designed to seamlessly integrate any website with the Firefly XD Path. It serves two key functions:

1. User behavior analytics - by tracking a user's behavior on the website (e.g. pageviews, time on page, sessions, etc), we're able to tie behavior from the website to down-funnel performance within the Firefly XD platform.

2. CTA connectivity - the tracking code automatically connects CTA buttons that drive to the Firefly XD Path with proper user identifiers to allow cross-domain analysis.

## How to install

Step 1:
At the bottom of the `<body>` tag include the following scripts on all pages:
```
<script type="text/javascript" src="https://fm.fireflyxd.io/3.1/fm-slim.js"></script>
<script type="text/javascript">
  fm.start( {
    current_path: 'brand',
    current_path_version: '1',
    program_id: 'addyi',
    program_version: 'client',
    page_name: window.location.pathname
  } );
</script>
```

Step 2: Add the destination URL and `className` to all CTA `<a>` elements that should drive to Firefly XD Path
```
<a href="https://addyi.fireflyxd.com" class="fm-link">Get X Now</a>
```
For any CTA link simply append the class name `fm-link` (i.e., `<a href="site.fireflyxd.com" class="fm-link">Link Out</a>`.

## Google Analytics Client ID Tracking
1. Follow steps 1 and 2.
2. Place your Google Analytics tracking script after fm is imported and started.

*`fm.router.append` will accept an object of key/value pairs and append any previously linked .fm-link anchor tags with the properteries and values in the object.*

```
<script type="text/javascript">
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-XXXXXXXXX-X', 'auto');
  ga(function(tracker) {
    fm.router.append({ ga_id: tracker.get('clientId') });
  });
</script>
```

## Alternate Attributions
In situations where other attributions are being used fm may be started as follows:
```
<script type="text/javascript">
  fm.start( {
    current_path: 'brand',
    current_path_version: '1',
    program_id: 'addyi',
    program_version: 'client',
    page_name: window.location.pathname,
    alt_source: 'Source name'
  } );
</script>
```
