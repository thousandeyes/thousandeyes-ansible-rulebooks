# Ansible Collection - thousandeyes.rulebooks

This collection contains example rulebooks for using ThousandEyes webhook alert notifications with 
Event-Driven Ansible. 

The example rulebooks are located in [`extensions/eda/rulebooks/`](extensions/eda/rulebooks):

   * `renew_tls_certificate.yml`: Demonstrates how to ingest a ThousandEyes alert notification for 
        TLS Certificate expiry
   * `restart_web_service.yml`: Demonstrates how to ingest a ThousandEyes alert notification for HTTP 
        5xx server error response

## Getting Started

To ingest ThousandEyes webhook alert notifications within an Event-Driven Ansible rulebook, first 
configure an `ansible.eda.webhook` event source in the rulebook:

```
---
- name: ThousandEyes Webhook Rulebook
  hosts: all
  sources:
   - ansible.eda.webhook:
       host: 0.0.0.0
       port: 8080

```

After configuring the webhook event source, define one or more rules which match on the webhook 
event. For example, the rules below demonstrate how to match a notification for an active alert 
notification versus a cleared alert notification:

```
  rules:
    - name: Active Alert 
      condition: event.payload.type == "2"   # Active alert type
      action:
        # ... 
    - name: Cleared Alert
      condition: event.payload.type == "1"   # Cleared alert type
      action:
        # ... 
```

You can also match on the ThousandEyes alert rule expression by using the 
`event.payload.alert.rule.expression` property in your rule, for example:

```
  rules:
    - name: High HTTP Response Time
      condition: event.payload.alert.rule.expression is match("(responseTime > 500 ms)", ignorecase=true) and event.payload.type == 2
      action:
        # ...
```

For detailed documentation, including how to create the Custom Webhook integration in the
ThousandEyes platform, please see the [Event-Driven Ansible for Alert Notifications](https://docs.thousandeyes.com/product-documentation/integration-guides/custom-webhook-examples/event-driven-ansible-for-alert-notifs)
article in the ThousandEyes documentation. 

## Contributing

For information on how to contribute to this repository, including reporting issues and sending pull 
requests, please see [`CONTRIBUTING.md`](CONTRIBUTING.md) in the root of the repository. 

## License

Use of this source code is governed by an MIT-style license that can be found in the [`LICENSE`](LICENSE) file 
in the root of the repository. 