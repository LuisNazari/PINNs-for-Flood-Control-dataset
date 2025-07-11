# PINNs-for-Flood-Control-dataset
This repository includes the dataset use for training PINNs for flood control in a Hydrographic Basin.

Considering the dynamics of unidimensional (or two-dimensional) water flow propagation in a single open channel without downstream inflow, the standard conceptual approach drives the flow rate and water level dynamics through the solution of the Saint-Venant equations \cite{cunge1980practical, chaudhry2007open}. The Saint-Venant equations \eqref{pbase:Saint_eq-1} and \eqref{pbase:Saint_eq-2} are two partial differential equations resulting from the physical principles of mass and momentum conservation, defined as follows:


\begin{subequations}\label{pbase:Saint_eq-12} 
\begin{equation}
    %%%
	\frac{\partial A(x,t)}{\partial h(x,t)}  \frac{\partial h(x,t)}{\partial t} + \frac{\partial Q(x,t)}{\partial x} = 0     \label{pbase:Saint_eq-1} 
\end{equation}
\begin{equation}
	\frac{\partial Q(x,t)}{\partial t} + \frac{\partial }{\partial x} \frac{ Q^2(x,t)}{ A(x,t)} +g A(x,t)\left( \frac{\partial h(x,t)}{\partial x} + S_f(x,t) - S_0 \right) = 0  \label{pbase:Saint_eq-2}
	  %%%
\end{equation}
\end{subequations}


where $t$ is the time variable (s); $x$ is the space variable which models the distance from the source (m); $Q(x,t)$ is the water flow (m$^3$/s); $h(x,t)$  is the water level (m); $A(x,t)$ is cross-sectional flow area (m$^2$); $g$ is the gravitational acceleration (m/s$^2$); $S_0$ is the bed slope; and $S_f $ is the friction slope, with the classical Manning formula:

 
\begin{equation}\label{eq:CoefManning}
   S_f(x,t) = \frac{n^2 Q(x,t)\left|Q(x,t)\right|}{A^2(x,t)R^{\frac{1}{3}}(x,t)}
\end{equation}


where $n$ is the Manning coefficient (m$^{-1/3}$s) and $R(x,t)$ is the hydraulic radius (m), defined by $R(x,t) = A(x,t)/P_w(x,t)$, with $P_w(x,t)$ defining the wetted perimeter (m)  \cite{litrico2009modeling}.

In order to train a PINN to model the water flow function $Q(x,t)$ and level $h(x,t)$, an appropriate database is required. The training data $U= \left\{\langle x^{i}_{u}, t^{i}_{u},h^{i}_{u}, Q^{i}_{u}\rangle \right\}^{N_u}_{i=1}$ can be obtained by numerical solution of PDE \eqref{eq:dynamicsimulated_Modeling} from the given initial and boundary conditions. To define the dataset, let us consider the dynamic system given by Equations \eqref{pbase:Saint_eq-1}-\eqref{pbase:Saint_eq-2} with boundary and initial conditions defined as:
    %%%
\begin{subequations}\label{eq:dynamicsimulated}
	\begin{align}
	& h(0,t) = \left\{\begin{array}{ll}
			h_{t_i}+\alpha_ie^{\frac{t_i - t}{1500}}\text{sin}\left(\frac{t-t_i}{450}\right) & \text{for } t \in (\Tilde{t}_i,\Tilde{t}_{i+1}], \ i = \{0,2,4,6,8\} \\
                h_{t_i} + \alpha_i\left(\frac{t - t_i}{300}\right)^2 & \text{for } t \in (\Tilde{t}_i,\Tilde{t}_{i+1}], \  i = \{1,3,5,7\}
			 \end{array}\right. \label{eq:boundarySimulated} \\
				%%%
			& Q(x,0) = \frac{(S_0)^{1/2}}{n} \cdot \frac{A^{5/3}(x,0)}{P^{2/3}(x,0)} \label{eq:inicialSimulated}
				%%%
	\end{align}
\end{subequations}

%
The analysis considers the one-dimensional propagation of the water flow from the initial condition defined by a stationary and uniform regime in Eq. \eqref{eq:inicialSimulated}, with $h(0,0) = h_{t_0}$ m, and the boundary conditions in Eq. \eqref{eq:boundarySimulated} parametrized by $\alpha_i$ with $i = \{0,1,2 \dots, 8 \}$ and $\Tilde{t}$ a partition of the time domain. For all channels, $\alpha_0 = 1.2$, $\alpha_1 = \alpha_5 = 0.02$, $\alpha_2 = \alpha_6 = \alpha_8 =1.5$, $\alpha_3 = -0.02$, and $\alpha_4 = -1.5$. The time domain is $[0,46000]$ seconds with partition $\Tilde{t} = [0,54,81,135,162,216,258,327,350,460] \times 10^{2}$. In contrast, the initial conditions differ and are defined as $h_{t_0} = [2,4,3,3]$ m for channels 1, 2, 3 and 4, respectively.

