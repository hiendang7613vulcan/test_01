Optimization Algorithms
├─ Continuous Optimization
│  ├─ Gradient-Based Methods
│  │  ├─ Deterministic (Unconstrained)
│  │  │  ├─ First-Order
│  │  │  │  └─ Gradient Descent (GD), Nesterov Accelerated Gradient (NAG), Nonlinear Conjugate Gradient (CG), Barzilai-Borwein (BB)
│  │  │  └─ Second-Order & Quasi-Newton
│  │  │     └─ Newton's Method (Line-Search/Trust-Region), Cubic Regularization, BFGS, L-BFGS, DFP, SR1
│  │  ├─ Deterministic (Constrained & Nonsmooth)
│  │  │  ├─ Primal-Dual & Interior-Point
│  │  │  │  └─ Interior-Point Methods (IPM), Primal-Dual IPM, Barrier Methods, Sequential Quadratic Programming (SQP)
│  │  │  ├─ Operator Splitting & Proximal
│  │  │  │  └─ Subgradient Methods, Proximal Point, Proximal Gradient (ISTA), FISTA, Bundle Methods, Augmented Lagrangian (ALM), ADMM, Douglas-Rachford, PDHG (Chambolle-Pock)
│  │  │  └─ Other Constrained
│  │  │     └─ Frank-Wolfe (Conditional Gradient), (Block) Coordinate Descent, Active-Set Methods
│  │  ├─ Stochastic (Large-Scale / ML)
│  │  │  ├─ Core & Adaptive Rates
│  │  │  │  └─ SGD, Momentum (Polyak, NAG-sc), AdaGrad, RMSProp, Adam, AdamW, NAdam, RAdam, AdaBelief, Adafactor, LARS/LAMB, Lion, Sophia, Adan, SM3, NovoGrad
│  │  │  ├─ Variance-Reduced
│  │  │  │  └─ SVRG, SAGA, SARAH, MISO, Katyusha, SPIDER, PAGE, SCSG
│  │  │  └─ Approximate Second-Order & Curvature
│  │  │     └─ (Block) Hessian-Free, K-FAC, Shampoo, AdaHessian, Sophia-G
│  │  └─ Sharpness-Aware Minimization
│  │     └─ SAM, GSAM, ASAM
│  └─ Derivative-Free (Black-Box) Methods (DFO)
│     ├─ Stochastic Approximation
│     │  └─ Robbins-Monro, Kiefer-Wolfowitz, SPSA (Simultaneous Perturbation)
│     ├─ Direct Search (Local & Model-Based)
│     │  └─ Nelder-Mead (Simplex), Pattern Search (Hooke-Jeeves), Powell's (NEWUOA, BOBYQA), COBYLA
│     ├─ Global Model-Based
│     │  ├─ Bayesian Optimization
│     │  │  └─ GP-UCB, EI, PI, TPE, SMAC, BOHB, TuRBO, SAASBO, LaMBO
│     │  └─ Other Global
│     │     └─ CMA-ES, DIRECT, Cross-Entropy Method
│     ├─ Population-Based Metaheuristics
│     │  └─ Genetic Algorithms (GA), Differential Evolution (DE), Particle Swarm Optimization (PSO), Ant Colony Optimization (ACO)
│     └─ Trajectory-Based Metaheuristics
│        └─ Simulated Annealing (SA), Tabu Search, Iterated Local Search (ILS), Variable Neighborhood Search (VNS), GRASP
├─ Discrete & Combinatorial Optimization
│  ├─ Exact Methods
│  │  ├─ (Mixed) Integer & Linear Programming
│  │  │  └─ Simplex Algorithm (LP), Interior-Point (LP), Branch-and-Bound, Branch-and-Cut, Branch-and-Price, Cutting-Plane Methods, Column Generation
│  │  └─ Other Exact
│  │     └─ Dynamic Programming (DP), Constraint Programming (CP), A* Search, SAT/SMT Solvers
│  ├─ Graph & Network Algorithms
│  │  ├─ Paths & Trees
│  │  │  └─ Dijkstra, A*, Bellman-Ford, Floyd-Warshall, Kruskal's, Prim's
│  │  └─ Flows & Matching
│  │     └─ Edmonds-Karp, Dinic's, Push-Relabel, Min-Cost Flow (Successive Shortest Path), Hungarian Algorithm, Blossom Algorithm
│  └─ Approximation Algorithms
│     └─ LP/SDP Relaxation & Rounding, Greedy (e.g., Christofides), Primal-Dual Schema, Randomized (e.g., Goemans-Williamson)
└─ Specialized Formulations & Settings
   ├─ Min-Max, Saddle-Point & Variational
   │  └─ Gradient-Based
   │     └─ Gradient Descent-Ascent (GDA), Extragradient (EG), Optimistic GDA (OGDA), Mirror-Prox, Tseng's Forward-Backward-Forward, Popov
   ├─ Online & Bandit Optimization
   │  ├─ Online Convex Optimization (OCO)
   │  │  └─ Online Gradient Descent (OGD), Follow-the-Regularized-Leader (FTRL), Online Mirror Descent (OMD), AdaHedge, Coin-Betting
   │  ├─ Stochastic & Adversarial Bandits
   │  │  └─ UCB, KL-UCB, Thompson Sampling, MOSS, EXP3, EXP3.S
   │  └─ Contextual Bandits
   │     └─ LinUCB, LinTS, NeuralUCB
   ├─ Distributed, Decentralized & Federated
   │  ├─ Centralized (Parameter Server)
   │  │  └─ Synchronous/Asynchronous SGD, Gradient Compression/Sparsification (QSGD, SignSGD, PowerSGD)
   │  ├─ Decentralized (Peer-to-Peer)
   │  │  └─ D-SGD, EXTRA, DIGing/Gradient Tracking, Push-Sum/Push-Pull (AB Methods), Exact Diffusion
   │  └─ Federated Learning (FL)
   │     └─ FedAvg, FedProx, SCAFFOLD, FedAdam/FedYogi, FedNova
   ├─ Multi-Objective Optimization (MOO)
   │  ├─ Classical & Decomposition
   │  │  └─ Weighted Sum, ε-Constraint, MOEA/D
   │  ├─ Evolutionary & Pareto-Based
   │  │  └─ NSGA-II, NSGA-III, SPEA2, SMS-EMOA
   │  └─ Bayesian (MOBO)
   │     └─ ParEGO, qEHVI, qNEHVI
   ├─ Robust & Distributionally Robust (RO & DRO)
   │  └─ Formulations & Solvers
   │     └─ Robust Counterpart (Set-based: Box, Ellipsoidal), Column-and-Constraint Generation (C&CG), DRO (Wasserstein/φ-divergence)
   ├─ Bilevel & Hyperparameter Optimization
   │  └─ Gradient-Based
   │     └─ Implicit Differentiation (AID/HOAG), Neumann Series, Reverse/Forward Differentiation (DARTS, iMAML)
   └─ Specialized Structures
      ├─ Nonlinear Least Squares
      │  └─ Gauss-Newton, Levenberg-Marquardt, Dogleg, Trust-Region Reflective
      └─ Conic & Optimal Transport
         └─ Conic Solvers (IPM for LP/SOCP/SDP), Entropic OT (Sinkhorn, Greenkhorn), IPOT, Bregman Projections
