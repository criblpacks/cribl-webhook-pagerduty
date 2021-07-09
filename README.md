# Cribl WebHook PagerDuty
----

This pack demonstrates use of the WebHook destination by using the PagerDuty service. You can use this function to alert your team of potential issues.

This pack watches [LogStream internal metrics](https://docs.cribl.io/docs/sources-cribl-internal) for indications that the persistent queue for destinations have started growing. This is easy enough to change for use with any data source you prefer.

**Note:** The WebHook destination is not intended to be used for streaming events en masse. In this pack we carefully restrict the potential outflow to single events.


## Requirements

Before you begin, ensure that you have met the following requirements:

* [An integration token (or tokens) from your PagerDuty account](https://support.pagerduty.com/docs/generating-api-keys)
* An output that has persistent queuing enabled
   - or adjust the drop filter and eval to meet your logs' needs

## Using The Pack

To use this Pack, follow these steps:

1. You'll need to enable Cribl Internal Metrics (or choose another data source)
2. You'll need to define a [WebHook destination](https://docs.cribl.io/docs/destinations-webhook) with the following settings:
   - URL: https://events.pagerduty.com/v2/enqueue
   - Method: POST
   - Format: NDJSON
   - Backpressure: User choice but we urge you to choose 'Drop'. We don't want a PagerDuty hiccup to block your workers.
   - Post-processing: Clear system fields
3. Add the integration token(s) to the lookup file under the Pack's Knowledge tab
    - You can specify a regex to match against host – use . to match anything if you only have 1 use case
4. Define a new route that points to the pack, with an output pointing to your webhook destination from step 2

The trigger_pipe pipeline trims your datastream down to just the events we're interested in for this demonstration: `cribl.logstream.pq.queue_size` with `value > 0`. For events that match, a WebHook will be sent to PagerDuty to trigger an incident up to once every 5 minutes per `host-output` combination. We also send `host-output` to PagerDuty for deduplication on their end.

Adjust these rules as required for your particular dataset and/or situation.

## Release Notes

### Version 1.0.0 - 2021-06-22
First release

## Contributing to the Pack
Contributions accepted via email. A github repo will be available soon.

## Contact
To contact us please email <jrust@cribl.io>.

## License
This Pack uses the following license: [`Apache 2.0`](https://github.com/criblio/appscope/blob/master/LICENSE).
