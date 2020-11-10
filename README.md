# apischema

grpc api schema



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

Pagination ...

```
        "chunking": {
            "pointer": 123,
            "perpage": 100
        },

        "chunking": {
            "pointer": 123,
            "alldata": 100
        },
```

Filter operators ...

```
and
any
bet
not
```
