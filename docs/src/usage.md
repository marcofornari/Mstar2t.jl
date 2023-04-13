# Usage

**Parallel version**: to run the code in parallel with Julia's multi-threading, launch a REPL specifying the number `N` of execution threads you need:
```bash
$ julia -t N

julia> Threads.nthreads() # to check threads available
N
```

Note: By running `julia -t auto`, Julia will use the number of local CPU threads.

## Examples
Check [examples](https://github.com/marcofornari/Mstar2t.jl/tree/main/examples) folder for examples.