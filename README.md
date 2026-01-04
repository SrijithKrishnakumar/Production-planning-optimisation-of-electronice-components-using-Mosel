Production Planning with Capacity Constraints and Overtime
This project develops a mixed‑integer linear programming model for multi‑product production planning under realistic capacity and overtime constraints, implemented in Xpress‑Mosel as part of the MSc in Business Analytics (MIS41160 Optimisation in Business) at University College Dublin. The model extends a classic textbook case to quantify the economic impact of limited capacity and to generate actionable recommendations for operations and capacity planning.
​

Business Context
Manufacturing firms often face fixed monthly capacity limits driven by workforce, machine time, and shift structures, and must rely on overtime when demand exceeds base capacity. Overtime supports service levels but increases unit cost and can reduce margins if not managed optimally.
​

This project addresses a realistic production planning problem where management must:
​

Satisfy product‑specific monthly demand over a 6‑month horizon.

Decide how much to produce in regular time vs overtime.

Manage inventory levels and production ramp‑ups/ramp‑downs.

Minimise total cost while respecting base and overtime capacity limits.
​

Problem Description
The use case is based on an extension of a standard production planning problem (Guéret et al., Xpress Applications, Problem 8.4), enhanced with capacity and overtime decisions.
​

Planning horizon: 6 months.

Products: 4 SKUs (e.g. X43‑M1, X43‑M2, Y54‑N1, Y54‑N2).
​

Demand: Deterministic monthly demand specified for each product.
​

Base capacity: 6,000 units/month across all products.

Overtime capacity: 1,500 units/month across all products.

Costs:

Regular production cost per product.

Overtime cost = regular production cost + €7 per unit premium.

Inventory holding cost per unit per month.

Cost of increasing or decreasing total production level between months.
​

Objective: Minimise total cost (production + overtime + inventory + change costs) while meeting demand and final stock targets.
​

Mathematical Model
The model is formulated as a linear optimisation problem with explicit capacity structure.
​

Decision variables
produce_base(p, t): Units of product 
p
p produced in month 
t
t within base capacity.
​

produce_overtime(p, t): Units of product 
p
p produced in month 
t
t as overtime.
​

produce(p, t): Total production = base + overtime.
​

store(p, t): Inventory of product 
p
p at end of month 
t
t.
​

add(t), reduce(t): Change in total production between months (increase/decrease).
​

Core constraints
Production definition:
produce
(
p
,
t
)
=
produce_base
(
p
,
t
)
+
produce_overtime
(
p
,
t
)
produce(p,t)=produce_base(p,t)+produce_overtime(p,t).
​

Capacity limits per month:

Base: 
∑
p
produce_base
(
p
,
t
)
≤
6000
∑ 
p
 produce_base(p,t)≤6000.

Overtime: 
∑
p
produce_overtime
(
p
,
t
)
≤
1500
∑ 
p
 produce_overtime(p,t)≤1500.
​

Inventory balance:
Initial stock + production − demand = ending stock each month, with final stock meeting product‑specific targets.
​

Production change:
Changes in total production are captured by add(t) and reduce(t) with associated costs, enforcing smooth transitions between months.
​

Non‑negativity: All decision variables are non‑negative.
​

Objective function
Minimise total cost:
​

Regular production cost: 
∑
p
,
t
C
P
R
O
D
(
p
)
⋅
p
r
o
d
u
c
e
_
b
a
s
e
(
p
,
t
)
∑ 
p,t
 CPROD(p)⋅produce_base(p,t).

Overtime cost: 
∑
p
,
t
(
C
P
R
O
D
(
p
)
+
7
)
⋅
p
r
o
d
u
c
e
_
o
v
e
r
t
i
m
e
(
p
,
t
)
∑ 
p,t
 (CPROD(p)+7)⋅produce_overtime(p,t).

Inventory cost: 
∑
p
,
t
C
S
T
O
C
K
(
p
)
⋅
s
t
o
r
e
(
p
,
t
)
∑ 
p,t
 CSTOCK(p)⋅store(p,t).

Change cost: 
∑
t
(
C
A
D
D
⋅
a
d
d
(
t
)
+
C
R
E
D
⋅
r
e
d
u
c
e
(
t
)
)
∑ 
t
 (CADD⋅add(t)+CRED⋅reduce(t)).
​

Implementation (Xpress‑Mosel)
The model is implemented in Xpress‑Mosel with clear separation of data, declarations, constraints, and reporting.
​

Key implementation features:

Arrays for demand, costs, and stock targets (DEM, CPROD, CSTOCK, ISTOCK, FSTOCK).
​

Explicit capacity parameters (BASE_CAPACITY, OVERTIME_CAPACITY, OVERTIME_COST_EXTRA).
​

Constraint blocks for capacity, stock balance, production changes, and final stock.
​

Comprehensive output reporting:

Base, overtime, and total production by product and month.

Capacity utilisation by month (base used, overtime used, totals vs capacity).

Stock levels, production changes, and a detailed cost breakdown.

Comparison of total cost vs the unlimited‑capacity base case (€683,929).
​

Results and Business Insights
Cost and capacity impact
Introducing capacity and overtime constraints increases total cost to approximately €715,498.75, a 4.6% rise (+€31,570) versus the unlimited‑capacity base case.
​

This quantifies the cost of constraint, providing a numeric basis for evaluating capacity expansion investments.
​

Capacity utilisation and overtime
Base production capacity (6,000 units/month) is fully utilised in all months, showing it is a binding constraint.
​

Overtime is heavily used in peak months (1–4), with utilisation around 71.5% of available overtime capacity, signalling reliance on premium‑cost production to meet demand.
​

Managerial recommendations
Short‑term:

Plan overtime proactively in high‑demand months with stable schedules to improve efficiency and workforce satisfaction.

Prioritise higher‑margin products when capacity is tight and use early production or flexible deliveries to smooth demand.
​

Medium‑term:

Use the €31,570 incremental cost as a benchmark when assessing ROI for capacity expansion or process improvements.

Identify bottlenecks that limit the 6,000‑unit base capacity and explore cross‑training to increase flexibility.
​

Long‑term:

Build a capacity expansion business case (e.g. 7,000–7,500 units base capacity) comparing total cost and service levels.
​

Enhance demand forecasting and consider automation to reduce marginal production and change costs.
