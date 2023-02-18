# Retention

To automatically delete data after a given threshold, set up a retention policy.

First connect to the InluxDb instance by running on the host/container:

```
$ influx
```

Then select a database and create a new retention policy:

```
USE home
CREATE RETENTION POLICY "two_weeks" ON "home" DURATION 2w REPLICATION 1 DEFAULT
```

In this example, the database is `home`, the name of the policy is `two_weeks` and data will be deleted after 2 weeks. The keyword `DEFAULT` ensures that the policy will be applied to all measurements, also those without an explicitly set retention policy.

Check the [InfluxDb docs](https://docs.influxdata.com/influxdb/v1.8/guides/downsample_and_retain/#2-create-a-two-hour-default-retention-policy) for more info.
