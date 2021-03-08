# apischema

This is the source of truth for the Venturemark gRPC API. The apischema defines
how RPC calls are shaped and function. Having a unified abstraction helps us
generate client and server code across different languages.



### Formatting

```
clang-format -i --style=google $(find ./pbf -name "*.proto")
```



### JSON-Patch

Complex updates require JSON-Patch operations. If a resource for instance
defines a list of strings it would require the whole list to be provided only in
order to add or remove a single item of that resource's list property. For such
operations the resource's service in question provides the option of JSON-Patch
operations.

- https://tools.ietf.org/html/rfc6902
- https://github.com/evanphx/json-patch
- https://github.com/Starcounter-Jack/JSON-Patch



### Operators

Filter operators we support are defined as follows.

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



### Visibility

Resource visibility may be defined via resource metadata. Below is shown how to
mark any resource as being publicly available.

```
"resource.venturemark.co/visibility": "public"
```

The equivalent for private resources may be similarly defined via resource
metadata. Below is shown how to mark any resource as being privately available.
Note that not defining the resource visibility metadata is equivalent to marking
a resource to be private.

```
"resource.venturemark.co/visibility": "private"
```
