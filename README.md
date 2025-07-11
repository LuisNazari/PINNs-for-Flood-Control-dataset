# PINNs-for-Flood-Control-dataset
This repository includes the dataset use for training PINNs for flood control in a Hydrographic Basin.

Considering the dynamics of unidimensional (or two-dimensional) water flow propagation in a single open channel without downstream inflow, the standard conceptual approach drives the flow rate and water level dynamics through the solution of the Saint-Venant equations [1][2]. The Saint-Venant equations 22a and 22b are two partial differential equations resulting from the physical principles of mass and momentum conservation, defined as follows:

<img width="718" height="159" alt="fig2" src="https://github.com/user-attachments/assets/9f9f8320-5c75-4c8c-b723-8c020b16b5b9" />

where $t$ is the time variable (s); $x$ is the space variable which models the distance from the source (m); $Q(x,t)$ is the water flow (m^3/s); $h(x,t)$  is the water level (m); $A(x,t)$ is cross-sectional flow area (m^2); $g$ is the gravitational acceleration (m/s^2); $S_0$ is the bed slope; and $S_f$ is the friction slope, with the classical Manning formula:

 <img width="597" height="80" alt="fig3" src="https://github.com/user-attachments/assets/343932a8-ae98-4b99-8f5f-80af70b841d4" />

where $n$ is the Manning coefficient (m^(-1/3)s) and $R(x,t)$ is the hydraulic radius (m), defined by $R(x,t) = A(x,t)/P_w(x,t)$, with $P_w(x,t)$ defining the wetted perimeter (m).

In order to train a PINN to model the water flow $Q(x,t)$ and level $h(x,t)$ functions, an appropriate database is required. The training data composed by $U= (x,t,h,Q)$ can be obtained by numerical solution of PDE (22) from the given initial and boundary conditions. To define the dataset, let us consider the dynamic system given by Equations 22a - 22b with boundary and initial conditions defined as:

<img width="815" height="166" alt="fig1" src="https://github.com/user-attachments/assets/c8dfc1d9-212c-4431-ad48-4b2998fc3412" />

The analysis considers the one-dimensional propagation of the water flow from the initial condition defined by a stationary and uniform regime in Eq. 35b, with $h(0,0) = h_{t_0}$ m, and the boundary conditions in Eq. 35a parametrized by $\alpha_i$ with $i = \{0,1,2 \dots, 8 \}$ and $\Tilde{t}$ a partition of the time domain. For all channels, $\alpha_0 = 1.2$, $\alpha_1 = \alpha_5 = 0.02$, $\alpha_2 = \alpha_6 = \alpha_8 =1.5$, $\alpha_3 = -0.02$, and $\alpha_4 = -1.5$. The time domain is $[0,46000]$ seconds with partition $\Tilde{t} = [0,54,81,135,162,216,258,327,350,460] \times 10^{2}$. In contrast, the initial conditions differ and are defined as $h_{t_0} = [2,4,3,3]$ m for channels 1, 2, 3 and 4, respectively.


REFERENCES

[1] CUNGE, Jean. Practical Aspects of Computational River Hydraulics. : Pitman Advanced Publishing Program, 1980


[2] CHAUDHRY, M Hanif. Open-Channel Flow. : Springer Science & Business Media, 2007.


[3] LITRICO, Xavier; FROMION, Vincent. Modeling and Control of Hydrosystems. Springer Science & Business Media, 2009.
