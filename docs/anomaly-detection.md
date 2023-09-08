# Anomaly detection

Anomaly detection is one of the primary functionalities of NetBeez. Fast and proactive detection is possible thanks to the real-time testing performed by the agents in conjunction with the alert detection process running on the BeezKeeper server. NetBeez users should familiarize themselves with the key concepts that are covered in this section. 

There are three main components of the NetBeez anomaly detection system:

- Alerts - Alerts are triggered by real-time tests based on certain conditions, as defined in alert detectors; alert detectors are assigned to targets.
    
- Incidents - Incidents are triggered by an agent, target, or WiFi network when a certain percentage of tests trigger alerts (incident threshold); incidents are a good way to reduce the noise from multiple alerts related to the same resource.
    
- Notifications - Notifications are delivered via email, Slack, SNMP traps, and other methods when an alert is triggered and/or an incident is raised.
    

![](https://lh3.googleusercontent.com/cLKIXf-OAsSO_mttC_bPo94_58Y133w9QGrir2TM4PPor_V4Bj41RuA_ydiwf8d8yUVBZFKU-5kFcUxB6OHhV7aeTDA7EGfWimPSbyJ5KFmzrYL7csmcBlrBPIB_AEsIUzswLw33O_jrUzZcqpYB7cw)

If the above chart is not clear, keep reading. In the next paragraphs, we’ll review in detail each one of these components.
## Alert profiles

Alert profiles are assigned to targets to detect problems such as loss of connectivity or performance degradation to a remote service or application. Alert profiles are test-specific, that is, are related to a specific type of test (ping, DNS, HTTP, traceroute, …). NetBeez offers default alert profiles, which can be edited or deleted. The user can create new alert profiles, and attach them to new or existing targets. Any change to an alert profile will be immediately pushed to all the targets where that profile has been enabled.

### Types of alert profiles

There are five types of alert profiles:

1. Up-Down - An up-down alert is triggered by a real-time test when it fails for a given number of consecutive tries. By default, the up-down multiplier is set to five. This value can be adjusted by the user at any time. 
    

![](https://lh5.googleusercontent.com/2Y1iTU97vMBhU7YEymPQ92y9WTLxRncxlb1ebJdET6IDRPET5X1Qcp-kiFsBZT0PFPVFZZc787Y6_qSwoN8qOSIIFfkZhtWLXPQFb9L_RzrxcA6XVVPyaXXqMAdcH8_F52CB9uCgDzGoBEZZMI8RmpU)

Up-down alerts are useful to detect loss of reachability to a remote host, network, or application.

2. Performance Baseline - A baseline alert is triggered by a real-time test when it detects a performance degradation issue. Performance degradation is detected by comparing the short-term moving average to its long-term, which is considered the performance baseline. In fact, for each test, the server calculates the following moving averages:
    

1. Short-term: 1 minute, 15 minutes, 1 hour, 4 hours.
    
2. Long-term: 1 day, 1 week, 1 month.
    

If the short-term average of a test is a certain number of times higher than its long-term average, an alert is triggered. 

![](https://lh4.googleusercontent.com/Ks3jdvyIGzod_0HwVFz8FqRyaPR7GVGIDfSHj8LmyybPlUYMlVfdowajc47WlP74Ss2fOlB92KV8d5X9IMUczNtNVPxdSM9TKD1uzV_oCs-EVsmG83JxQVIpVuPKzAdPRG3V4pbwiFdTa8PuRd_hs8k)

This type of alert profile is suited when a target is applied to many agents that have different performance results against the same application, due to their geographical location or other factors.

3. Performance Watermark - Watermark alerts are triggered when a real-time test doesn’t meet specific performance requirements. These alerts are used to enforce service level agreements with network services and applications. Watermark alerts are configured by comparing a short-term average against a user-defined threshold (eg. packet loss is higher than 5% or DNS resolution time is higher than 100 ms).
    

![](https://lh5.googleusercontent.com/4HDkntP6SNytDj9_3vpzHYpwNWBlRau1Kbs3g6c7yU2IHU2cR2G0_A5-iok33TA96uX1IJ8ocD9wZT0GgoBa-UjmKHP3o3VRVJoOQIc9ilF5S2bqY1T0oAkNert-XaF-j5XFhXO7spRy0ltNj3WqmJk)

4. Performance Baseline AND Watermark - This alert profile merges both performance baseline and watermark rules. If both detection methods are satisfied, then an alert will be triggered.
    
5. Down-Up - Down-up alerts are triggered when a test succeeds. The mechanism is the reciprocal of the up-down alert. Down-up alerts are used to enforce security policies, such as verifying content filtering (e.g. users can’t access certain websites) or firewall rules (e.g. an isolated network can’t access the Internet).
    
### Percentile-based mean

When calculating an average for a given time period, all test results are accounted for. However, especially in stable time series, one or a few outliers could dramatically skew the mathematical average, like in the below example. When this happens, performance alerts may be triggered, causing false positives.

![](https://lh3.googleusercontent.com/_wlwggP8Cpw-KCylIJ5v_Ztgc84dXRRCQb2x8Z-ThBZnWYK15bWexuTOyV64jHc3hvQe4TNMQXK9G75at9tyq11wlKbcSDqLwfQ1b-ukhMEwnJYiZQiLYi3pzjgv-B8x8S9LM5Le5LyvBsu5cIGNjhM)

NetBeez supports the use of a percentile-based mean (95th percentile), which filters out outliers that fall two times outside the standard deviation interval (from the mathematical average). The benefit of this function is that should a single data point skew the mathematical average, the percentile-based mean won't be affected, thus reducing the number of false positives (alerts noise). To enable this feature, the user can enable the use of a percentile-based mean in the [Anomaly Detection settings](https://docs.google.com/document/d/1GsIWkWI3mMj2xqG0Ce_1BNrb8t8RhePK_bokT24sjo4/edit#heading=h.n6ogakcoekwr) section.

![](https://lh4.googleusercontent.com/cpld4sE4c9AycXRcFzE-_IEOVL0IPBfS19i283YnUUaQX_QE7hns7QLG80Tifj2iip2aIXNh_s9O3uibXwJvuw0uwtAlCNZ58XGixAQYwtj3HVKJy0kIlgXqkR2YBnIYYwLqXy0tfDaHVyRuCiqNYD4)

If you want to learn more about alert profiles and percentile-based mean, please refer to the [online documentation page](https://netbeez.zendesk.com/hc/en-us/articles/201580529-Alerts-Configuration).

## Device alerts

Device alerts are triggered when an agent is either unreachable from the dashboard, or a WiFi agent isn’t able to connect to a WiFi network for a given period of time.

![](https://lh4.googleusercontent.com/GxKokyiWcoxG0xNUBRWASVs1iwKbW7-GNh-P6l_JB6AImHQuTc9p0gZINwbIvnoTXBfKen4AKPBHjf76_JcTUL_g_ZZzIKFJ6aBBJF39OhMQsxTG1TGQLT5JoAbnmLjo_pVZgY17xnDc2hGUs2v6FS8)

One important thing to remember is that, when a device is not connected to the dashboard anymore, all tests are placed in unknown status (marked with “?”), incidents are cleared and so are alerts.

## Incidents

Incidents are periods of degraded or otherwise abnormal performance of an agent, target, or WiFi network. This functionality is designed to help users identify problems and performance variations with a network location (agent), service or application (target), and WiFi network. Another benefit of incidents is that they reduce the number of notifications that a user has to receive.

An incident is triggered when a certain percentage of tests within one agent, target, or WiFi network trigger an alert. Such thresholds are defined by the user in the NetBeez “Incidents Configuration” settings, under the “Anomaly Detection” section.

![](https://lh6.googleusercontent.com/QL9vSo2pYnfVH_HRqdaPNKQ_BQJHOBfUehGERaJHI90gI9nqmXgsjn6DrHvyDBefm1CmFUzANj5oq7DrMdH4N66Nr1DscXAgauDRiM9PEm4F3PnJzxaVA85BT5EWAMa8dWsmjt9GEQywfz5oFmc6c88)

Incidents can be acknowledged, and users can post comments to include more information about the undergoing performance issue, or explain the reason why a specific incident was acknowledged or de-acknowledged.

If you want to read more about Incidents, please consult [this documentation page](https://netbeez.zendesk.com/hc/en-us/articles/115003579411-Incidents).
## Notifications

Even when not in front of the dashboard, users can receive notifications on alerts and incidents. NetBeez supports different delivery methods for notifying users about new alerts and incidents raised and closed, such as SNMP traps, Syslog messages, and emails. For each delivery method, the user can pick what to receive: only alerts, only incidents, or both of them. In general, it’s good practice to enable notifications for agent incidents and alerts, target incidents, and wifi incidents.

![](https://lh6.googleusercontent.com/9MtDcqzpN2CkVbjDo2fVlaRnoU0DrdbDd9W-0tebXEHG1OefbMLay4zDezOmlS79r3qCS8fuz_Vn6ZV-dxSUeDeKJpQWzf7fRK1jS_w5MRxh0sgg-CM9GNLc_Yv18JYdJOjcFhY99Bk3JxzeveCZFMU)

NetBeez also provides integrations with many third-party tools, such as Splunk, PagerDuty, and Slack. You can learn more about these integrations on the [online documentation](https://netbeez.zendesk.com/hc/en-us/sections/201825346-Integrations-and-API).

### Data retention

NetBeez supports user-defined data retention settings. The user can set for how long the central server should retain performance data collected by the agents. In practice, data retention dictates how far back historical test data and reports data can go. Some of the variables to be set are:

- Raw results - This is the raw data from test results, where each data point is the result of a test. Raw test data is displayed in real-time and historical graphs.
    
- 1-min average - This is the average of test results collected in one minute. The 1-min average is used to generate performance alerts and reports.
    
- 1-hour average - This is the average of test results collected in one hour. The 1-hour average is used to generate performance alerts and reports.
    
- 24-hour average  - This is the average of test results collected in twenty-four hours. The 24-hour average is used to generate performance alerts and reports.
    
The resulting disk space required is dependent on the time period selected, the number of tests, and their interval. This configuration setting can be easily applied from the NetBeez Settings in a few clicks. 

![](https://lh4.googleusercontent.com/9yektIgjhor1RwxB6Qgd0wkwPuzQcD5kXnKveTeRAT0TOsqZlw5J5puJIksUfbqzIxkDiztwMvpNJpZ-E5LRjNBMyZUg4wWSKX82BFlSMUWVO4836M4FTwMXs-SW0LGGgJnrWfezkorbX2OXVUZLmx0)

Please refer to the [online documentation page](https://netbeez.zendesk.com/hc/en-us/articles/201582539-Settings-Data-Retention) to learn more about this.