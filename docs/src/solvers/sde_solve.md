# SDE Solvers

## Recommended Methods

For most problems where a good amount of accuracy is required and stiffness may
be an issue, the `SRIW1Optimized` algorithm should do well. If the problem
has additive noise, then `SRA1Optimized` will be the optimal algorithm. If
you simply need to quickly compute a large ensamble and don't need accuracy
(and don't have stiffness problems), then `EM` can do well.

## Special Keyword Arguments

* `save_noise`: Determines whether the values of `W` are saved whenever the timeseries
  is saved. Defaults to true.
* `delta`: The `delta` adaptivity parameter for the natural error estimator. For
  more details, see [the publication](http://chrisrackauckas.com/assets/Papers/ChrisRackauckas-AdaptiveSRK.pdf).

## Implemented Solvers

### StochasticDiffEq.jl

Each of the StochasticDiffEq.jl solvers come with a linear interpolation.

- `EM`- The Euler-Maruyama method.
- `RKMil` - An explicit Runge-Kutta discretization of the strong Order 1.0 Milstein method.
- `SRA` - The strong Order 2.0 methods for additive SDEs due to Rossler. Not yet implemented.
  Default tableau is for SRA1.
- `SRI` - The strong Order 1.5 methods for diagonal/scalar SDEs due to Rossler.
  Default tableau is for SRIW1.
- `SRIW1` - An optimized version of SRIW1. Strong Order 1.5.
- `SRA1` - An optimized version of SRIA1. Strong Order 2.0.

For `SRA` and `SRI`, the following option is allowed:

* `tableau`: The tableau for an `:SRA` or `:SRI` algorithm. Defaults to SRIW1 or SRA1.

#### StochasticCompositeAlgorithm

One unique feature of StochasticDiffEq.jl is the `StochasticCompositeAlgorithm`, which allows
you to, with very minimal overhead, design a multimethod which switches between
chosen algorithms as needed. The syntax is `StochasticCompositeAlgorithm(algtup,choice_function)`
where `algtup` is a tuple of StochasticDiffEq.jl algorithms, and `choice_function`
is a function which declares which method to use in the following step. For example,
we can design a multimethod which uses `EM()` but switches to `RKMil()` whenever
`dt` is too small:

```julia
choice_function(integrator) = (Int(integrator.dt<0.001) + 1)
alg_switch = StochasticCompositeAlgorithm((EM(),RKMil()),choice_function)
```

The `choice_function` takes in an `integrator` and thus all of the features
available in the [Integrator Interface](@ref)
can be used in the choice function.

### Adaptive Type: RSWM

Algorithms which allow for adaptive timestepping (all except `EM` and `RKMil`)
can take in an `RSWM` type which specifies the rejction sampling with memory
algorithm used. The constructor is:

```julia
RSWM(;discard_length=1e-15,
     adaptivealg::Symbol=:RSwM3)
```

* `discard_length` - Size at which to discard future information in adaptive. Default is 1e-15.
* `adaptivealg`: The adaptive timestepping algorithm. Default is `:RSwm3`.

For more details, see [the publication](http://chrisrackauckas.com/assets/Papers/ChrisRackauckas-AdaptiveSRK.pdf).
