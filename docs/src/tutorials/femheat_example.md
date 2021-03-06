# Heat Equation Finite Element Method

This tutorial will introduce you to the functionality for solving a PDE. Other
introductions can be found by [checking out DiffEqTutorials.jl](https://github.com/JuliaDiffEq/DiffEqTutorials.jl).

In this example we will solve the heat equation ``u_t=Δu+f``. To do this, we define
a HeatProblem which contains the function ``f`` and the boundary conditions. We
specify one as follows:

```julia
f(t,x,u)  = ones(size(x,1)) - .5u
u0_func(x) = zeros(size(x,1))
```

Here the equation we chose was nonlinear since ``f`` depends on the variable ``u``.
Thus we specify `f=f(u,x,t)`. If ``f`` did not depend on u, then we would specify f=f(x,t).
We do need to specify ``gD`` (the Dirichlet boundary condition) and ``gN`` (the Neumann
boundary condition) since both are zero. ``u_0`` specifies the initial condition. These together
give a HeatProblem object which holds everything that specifies a Heat Equation Problem.

We then generate a mesh. We will solve the equation on the parabolic cylinder
``[0,1]^2 \times [0,1]``. You can think of this as the cube, or at every time point from 0
to 1, the domain is the unit square. To generate this mesh, we use the command

```julia
tspan = (0.0,1.0)
dx = 1//2^(3)
dt = 1//2^(7)
mesh = parabolic_squaremesh([0 1 0 1],dx,dt,tspan,:neumann)
u0 = u0_func(mesh.node)
prob = HeatProblem(u0,f,mesh)
```  

Notice that here we used the mesh to generate our `u0` from a function which specifies
`u0`. We then call the solver

```julia
sol = solve(prob,FEMDiffEqHeatImplicitEuler())
```

Here we have chosen to use the ImplicitEuler algorithm to solve the equation. Other algorithms
and their descriptions can be found in the solvers part of the documentation.
