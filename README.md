# apischema

This is the source of truth for the Venturemark gRPC API. The apischema defines
how RPC calls are shaped and function. Having a unified abstraction helps us
generate client and server code across different languages.



### Search

The search primitives provided for any RPC call work by filtering based on
relevant properties, which the queried data objects define. Additionally there
is support for logical operators to further specify the data lookup. E.g.
searching for a single metric can be done by filtering for the metric ID, like
shown below. Note that in this example no operator must be provided, because
only a single property is provided for filtering. The response returned does
then only contain a single item in the resulting list, since there does only one
metric object exist having the provided metric ID.

```
{
    "filter": {
        "property": [
            {
                "metric_id": "met-d289g"
            }
        ]
    }
}
```

Searching for all metrics of several updates can be done by filtering for the
corresponding update IDs like shown below. Note that an operator must be
provided in case multiple properties are filtered for. Here the operator `any`
means that all metrics of each update should be returned. The response returned
does then contain all metric objects being associated with the provided update
IDs.

```
{
    "filter": {
        "operator": [
            "any",
        ],
        "property": [
            {
                "update_id": "upd-j3o45"
            },
            {
                "update_id": "upd-kj34n"
            },
            {
                "update_id": "upd-9snjs"
            }
        ]
    }
}
```

Searching for all metrics of a particular update while restricting the timeframe
in which the metrics got created can be done by filtering for the corresponding
update ID and the relevant timestamps like shown below. Note that an operator
must be provided in case multiple properties are filtered for. Here the operator
`bet` means that all metrics created within the given timestamps should be
returned. The response returned does then contain all metric objects being
associated with the provided update ID and timestamps.

```
{
    "filter": {
        "operator": [
            "bet",
        ],
        "property": [
            {
                "timestamp": "1604959525",
                "update_id": "upd-9snjs"
            },
            {
                "timestamp": "1605025038",
                "update_id": "upd-9snjs"
            }
        ]
    }
}
```



### Operators

Filter operators we support are defined as follows.

- `and` expresses that all properties must match within requested data objects
- `any` expresses that any property can match within requested data objects
- `bet` expresses that only properties within the given range must match within
  requested data objects
- `not` expresses that no property as given must be represented within requested
  data objects



### Pagination

Pagination is not yet supported, though the specification for splitting up
responses of larger results would work as follows. Along with `filter.operator`
and `filter.property` there can `filter.chunking` be specified. When sending
requests the additional input configuration would look like the example below.
In the example below the response would contain the first batch of 100 items.
Consecutive calls would require the pointer returned with the preceding response
in order to continue fetching the next batches accordingly.

```
{
    "filter": {
        "chunking": {
            "pointer": 0,
            "perpage": 100
        },
        ...
    }
}
```

With pagination the response would as well contain `filter.chunking`. The
additional chunking information indicates the pointer to be used for requesting
the next batch of items as well as the total amount of data to be expected when
continuing to request all available batches of data objects. When receiving
search responses there will always be the `result` key containing a list of
requested data objects.

```
{
    "filter": {
        "chunking": {
            "pointer": 123,
            "intotal": 750
        }
    },
    "result": [
        { ... },
        { ... },
        { ... }
    ]
}
```
