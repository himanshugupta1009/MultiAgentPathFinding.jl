# MultiAgentPathFinding.jl

A Julia implementation of two fundamental Multi-Agent Path Finding (MAPF) algorithms -
[Conflict-Based Search](https://www.sciencedirect.com/science/article/pii/S0004370214001386) or CBS,
and its bounded-suboptimal variant, [Enhanced CBS](https://www.aaai.org/ocs/index.php/SOCS/SOCS14/paper/view/8911).
This repository is heavily based on a [C++ library](https://github.com/whoenig/libMultiRobotPlanning) for multi-robot planning
(with comparable performance - see below).


## If you do want to use this...
The MultiAgentPathFinding repository is set up as a package with its own environment in [Julia 1.0](https://julialang.org/downloads/). Look at **Using someone else's project** at the Julia [package manager documentation](https://julialang.github.io/Pkg.jl/v1/environments/#Using-someone-else's-project-1) for the basic idea. To get the code up and running (after having installed Julia), first `cd` into the `MultiAgentPathFinding` folder.
Then start the Julia REPL and go into [package manager](https://julialang.github.io/Pkg.jl/v1/getting-started/) mode by pressing `]`, followed by:
```shell
(v1.0) pkg> activate .
(MultiAgentPathFinding) pkg> instantiate
```
This will install the necessary dependencies and essentially reproduce the Julia environment required to make the package work. You can test this by exiting the package manager mode with the backspace key and then in the Julia REPL entering:
```shell
julia> using MultiAgentPathFinding
```
The full package should then pre-compile.

### Running an Example
The `scripts/` folder has an example for each of CBS and ECBS on the 2D Grid World domain (a standard benchmark for MAPF algorithms - see the ECBS paper).
The `main` function in each script has an `infile::String` argument.
The infiles here refer to the files in the `benchmark` folder.
**Please Note**, you need to use JSON versions of the YAML files that are present in the folder. 

Once you've done all that, running an example is pretty easy. Assuming you have a `benchmark/` folder at the top-level, you can do the following (while having the environment activated):
```shell
julia> include("scripts/cbs_grid2d_example.jl")
julia> main("./benchmark/<your-env-filename>.json", "<some-out-file>.json", "SOC")
```
where `"SOC"` refers to the sum-of-costs high-level objective (could also be `"MS"` for makespan).
The call to `main` also outputs the time required for CBS through the `@time` macros. You have to discard the timing from the first call to `main` as it triggers compilation and the [timing is higher than the true one](https://docs.julialang.org/en/v1/manual/performance-tips/index.html#Measure-performance-with-[@time](@ref)-and-pay-attention-to-memory-allocation-1).

You can visualize the output solution file by using `scripts/visualize.py` (which has been [copied over](https://github.com/whoenig/libMultiRobotPlanning/blob/master/example/visualize.py) from the C++ reference repository).


```shell
python3 scripts/visualize.py --map ./benchmark/<your-env-filename>.json --schedule <some-out-file>.json
```
