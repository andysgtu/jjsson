# JSON Parser

a simple lightweight json parser 

## Install

```
make install
source enable
```
will install the header files and the shared library. The installation directory is `install`
by default, to change it use `make PREFIX=/path`.

The Bison+Flex implementation is not included but can be compiled with `make gnu/check`.

## Build Upon It

After making sure that the library has been sourced (`source enable`)
you can easily link it to your own tool:

```
g++ YourTool.cpp $(pkg-config --cflags --libs jpaser)
```

## API Examples

For a complete use case check out [`tools/Pretty.cpp`](tools/Pretty.cpp).

You can implement customized visitors by extending the class `Visitor`.

```c++
json_parser::Object object;
if (object.From("{\"mixed\": [1,2.3,"four"]")) {
  PrettyVisitor pv;
  object.Accept(pv);
  cout << pv.GetResult();
}
```
will print
```json
{
    "mixed": [
        1,
        2.3,
        "four"
    ]
}
```

Try implementing a your variant that does something more special!

## Benchmarking

To run a parsing benchmark on the set of JSON files provided in `data/benchmark` use `make benchmark`.
The command uses `jmatch` as the default program during the benchmark,
to set another one set the `CMD` make variable.

```
source enable
cd benchmark
make
```

The command will produce an output like the following:
```
file                              size[MB]  time[s]  speed[MB/s]
data/benchmark/canada.json        2.146     .084     25.547
data/benchmark/citm_catalog.json  1.647     .046     35.804
data/benchmark/twitter.json       .602      .021     28.666
```
Try comparing it against the popular JSON processor!
```
make benchmark CMD=jq
```

## Testing

To run a compliance test:
```
make test
```
- Files in `tests/fail` are supposed to be rejected.
- Files in `tests/pass` are supposed to be accepted.

These files have been taken from [json.org](http://json.org/JSON_checker/).

See also [Native JSON Benchmark](https://github.com/miloyip/nativejson-benchmark) for more
information.
