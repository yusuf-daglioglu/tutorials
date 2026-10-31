# MONITORING TOOLS AND ACTUATORS REQUEST EXAMPLES

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

IIIIIIIIIIIIIIIIIIIIIIIIIIIIIIII

## 📌 OTel Request examples

All examples copied from:

https://github.com/open-telemetry/opentelemetry-proto/tree/main/examples

## 📌 metric example

https://github.com/open-telemetry/opentelemetry-proto/blob/main/examples/metrics.json

```json
{
  "resourceMetrics": [
    {
      "resource": {
        "attributes": [
          {
            "key": "service.name",
            "value": {
              "stringValue": "my.service"
            }
          }
        ]
      },
      "scopeMetrics": [
        {
          "scope": {
            "name": "my.library",
            "version": "1.0.0",
            "attributes": [
              {
                "key": "my.scope.attribute",
                "value": {
                  "stringValue": "some scope attribute"
                }
              }
            ]
          },
          "metrics": [
            {
              "name": "my.counter",
              "unit": "1",
              "description": "I am a Counter",
              "sum": {
                "aggregationTemporality": 1,
                "isMonotonic": true,
                "dataPoints": [
                  {
                    "asDouble": 5,
                    "startTimeUnixNano": "1544712660300000000",
                    "timeUnixNano": "1544712660300000000",
                    "attributes": [
                      {
                        "key": "my.counter.attr",
                        "value": {
                          "stringValue": "some value"
                        }
                      }
                    ]
                  }
                ]
              }
            },
            {
              "name": "my.gauge",
              "unit": "1",
              "description": "I am a Gauge",
              "gauge": {
                "dataPoints": [
                  {
                    "asDouble": 10,
                    "timeUnixNano": "1544712660300000000",
                    "attributes": [
                      {
                        "key": "my.gauge.attr",
                        "value": {
                          "stringValue": "some value"
                        }
                      }
                    ]
                  }
                ]
              }
            },
            {
              "name": "my.histogram",
              "unit": "1",
              "description": "I am a Histogram",
              "histogram": {
                "aggregationTemporality": 1,
                "dataPoints": [
                  {
                    "startTimeUnixNano": "1544712660300000000",
                    "timeUnixNano": "1544712660300000000",
                    "count": "2",
                    "sum": 2,
                    "bucketCounts": ["1","1"],
                    "explicitBounds": [1],
                    "min": 0,
                    "max": 2,
                    "attributes": [
                      {
                        "key": "my.histogram.attr",
                        "value": {
                          "stringValue": "some value"
                        }
                      }
                    ]
                  }
                ]
              }
            },
            {
              "name": "my.exponential.histogram",
              "unit": "1",
              "description": "I am an Exponential Histogram",
              "exponentialHistogram": {
                "aggregationTemporality": 1,
                "dataPoints": [
                  {
                    "startTimeUnixNano": "1544712660300000000",
                    "timeUnixNano": "1544712660300000000",
                    "count": "3",
                    "sum": 10,
                    "scale": 0,
                    "zeroCount": "1",
                    "positive": {
                      "offset": 1,
                      "bucketCounts": ["0","2"]
                    },
                    "min": 0,
                    "max": 5,
                    "zeroThreshold": 0,
                    "attributes": [
                      {
                        "key": "my.exponential.histogram.attr",
                        "value": {
                          "stringValue": "some value"
                        }
                      }
                    ]
                  }
                ]
              }
            }
          ]
        }
      ]
    }
  ]
}
```

Yukarıdaki `metric`'in sadece ilki (`my.counter`), `Prometheus Text format`'ında şuna denk gelmektedir:

```text
my.counter{service.name="my.service",my.counter.attr="some value"} 5 1544712660300000000
```

`time series` storage'de nasıl tutulduğu başka başlıkta anlatılıyor. `JSON`'daki bazı alanlar (`isMonotonic`, `unit`, `aggregationTemporality` gibi) bu kapsamın dışında kaldığı için onlar `time series` storage'de metadata dosyasında tutuluyor.

## 📌 trace example

https://github.com/open-telemetry/opentelemetry-proto/blob/main/examples/trace.json

```json
{
  "resourceSpans": [
    {
      "resource": {
        "attributes": [
          {
            "key": "service.name",
            "value": {
              "stringValue": "my.service"
            }
          }
        ]
      },
      "scopeSpans": [
        {
          "scope": {
            "name": "my.library",
            "version": "1.0.0",
            "attributes": [
              {
                "key": "my.scope.attribute",
                "value": {
                  "stringValue": "some scope attribute"
                }
              }
            ]
          },
          "spans": [
            {
              "traceId": "5B8EFFF798038103D269B633813FC60C",
              "spanId": "EEE19B7EC3C1B174",
              "parentSpanId": "EEE19B7EC3C1B173",
              "name": "I'm a server span",
              "startTimeUnixNano": "1544712660000000000",
              "endTimeUnixNano": "1544712661000000000",
              "kind": 2,
              "attributes": [
                {
                  "key": "my.span.attr",
                  "value": {
                    "stringValue": "some value"
                  }
                }
              ]
            }
          ]
        }
      ]
    }
  ]
}
```

## 📌 event example

https://github.com/open-telemetry/opentelemetry-proto/blob/main/examples/events.json

```json
{
  "resourceLogs": [
    {
      "resource": {
        "attributes": [
          {
            "key": "service.name",
            "value": {
              "stringValue": "my.service"
            }
          }
        ]
      },
      "scopeLogs": [
        {
          "scope": {
            "name": "my.library",
            "version": "1.0.0",
            "attributes": [
              {
                "key": "my.scope.attribute",
                "value": {
                  "stringValue": "some scope attribute"
                }
              }
            ]
          },
          "logRecords": [
            {
              "eventName": "browser.page_view",
              "timeUnixNano": "1544712660300000000",
              "observedTimeUnixNano": "1544712660300000000",
              "severityNumber": 9,
              "severityText": "test severity text",
              "attributes": [
                {
                  "key": "event.attribute",
                  "value": {
                    "stringValue": "some event attribute"
                  }
                }
              ],
              "body": {
                "kvlistValue": {
                  "values": [
                    {
                      "key": "type",
                      "value": {
                        "intValue": "0"
                      }
                    },
                    {
                      "key": "url",
                      "value": {
                        "stringValue": "https://www.guidgenerator.com/online-guid-generator.aspx"
                      }
                    },
                    {
                      "key": "referrer",
                      "value": {
                        "stringValue": "https://wwww.google.com"
                      }
                    },
                    {
                      "key": "title",
                      "value": {
                        "stringValue": "Free Online GUID Generator"
                      }
                    }
                  ]
                }
              }
            }
          ]
        }
      ]
    }
  ]
}
```