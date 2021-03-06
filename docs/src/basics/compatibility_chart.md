# Solver Compatibility Chart

This chart is for documenting the compatibility of the component solver packages
to the common interface. An `x` means that the option is implemented or the
add-on functionality will work with the given solver. A blank means that
the option has not been implemented or that a given add-on has not been tested
with a given package. If there are any errors in this chart, please file an
issue or submit a pull-request.

| Option                                 | OrdinaryDiffEq.jl | Sundials.jl | ODE.jl | ODEInterface.jl | LSODA.jl | StochasticDiffEq.jl | DelayDiffEq.jl | DASKR.jl | DASSL.jl | ODEIterators.jl |
|----------------------------------------|-------------------|-------------|--------|-----------------|----------|---------------------|----------------|----------|----------|-----------------|
| Dense (continuous) output              | x                 |             | x      |                 |          | x                   | x              |          |          |                 |
| Tolerance control                      | x                 | x           | x      | x               | x        | x                   | x              | x        | x        | x               |
| Advanced stepsize control              | x                 |             |        | x               |          | x                   | x              |          |          |                 |
| internalnorm                           | x                 |             | x      |                 |          | x                   | x              |          |          |                 |
| Initial dt                             | x                 |             | x      | x               |          | x                   | x              |          |          |                 |
| save_timeseries                        | x                 | x           | x      |                 | x        | x                   | x              | x        |          |                 |
| timeseries_steps                       | x                 |             |        |                 |          | x                   | x              |          |          |                 |
| saveat                                 | x                 | x           | x      |                 | x        | x                   | x              | x        |          |                 |
| tstops                                 | x                 |             |        |                 |          | x                   | x              |          |          |                 |
| d_discontinuities                      | x                 |             |        |                 |          | x                   | x              |          |          |                 |
| isoutofdomain                          | x                 |             | x      |                 |          | x                   | x              |          |          |                 |
| Allows reverse time direction          | x                 | x           | x      | x               | x        | x                   | x              |          |          |                 |
| Unitful numbers                        | x                 |             |        |                 |          |                     | x              |          |          |                 |
| Arbitrary dimension arrays             | x                 | x           | x      | x               | x        | x                   | x              | x        | x        | x               |
| Complex numbers                        | p                 |             |        |                 |          | x                   | p              |          |          |                 |
| Arbitrary precision                    | x                 |             | x      |                 |          | x                   | x              |          | x        |                 |
| ApproxFun types                        | x                 |             |        |                 |          |                     | x              |          |          |                 |
| Progress monitoring                    | x                 |             |        |                 |          | x                   | x              |          |          |                 |
| Integrator interface                   | x                 |             |        |                 |          | x                   | x              |          |          |                 |
| Resizability                           | x                 |             |        |                 |          | x                   | x              |          |          |                 |
| Cache iterator                         | x                 |             |        |                 |          | x                   | x              |          |          |                 |
| Can choose linear solvers              | x                 | s           |        |                 |          | x                   | x              |          | x        |                 |
| Can choose nonlinear solvers           |                   |             |        |                 |          |                     |                |          | x        | x               |
| Can use out of place                   | x                 |             | x      |                 |          | x                   | x              |          | x        | x               |
| Can use inplace                        | x                 | x           |        | x               | x        | x                   | x              | x        | x        | x               |
| Compatible with DiffEqDevTools         | x                 | x           | x      | x               | x        | x                   | x              | x        |          |                 |
| Compatible with ParameterizedFunctions | x                 | x           | x      | x               | x        | x                   | x              | x        |          |                 |
| Continuous Callbacks                   | x                 |             |        |                 |          | x                   | x              |          | x        | x               |
| Discrete Callbacks                     | x                 |             |        |                 |          | x                   | x              |          |          | x               |
| Monte Carlo Simulations                | x                 | x           | x      | x               | x        | x                   | x              | x        |          | x               |
| Parameter Estimation                   | x                 | n           | n      | n               | n        |                     | x              |          | x        | x               |
| Parameter Sensitivity Analysis         | x                 | x           | x      | x               | x        |                     | x              |          |          |                 |
| Plotting and solution handling         | x                 | x           | x      | x               | x        | x                   | x              | x        |          |                 |

* p: partial compatibility, only in nonstiff methods unless the Jacobian is provided.
* n: general compatibility, but not compatible with routines which.
  require being able to autodifferentiate through the entire solver.
* s: Special, Sundials has its own linear solver choices.

## Note on PDEs

This chart is only for the basic (ODE/SDE/DAE/DDE) solver methods. The PDE
solvers (will be) built on top of these packages and thus will have the same
options available. Current, FiniteElementDiffEq.jl  is a solo implemention
which is compatible with `save_timeseries`, `timeseries_steps`, plotting and
solution handling, arbitrary precision, and choice of `dt`.
