---
title: 'Information relaxation'
date: 2022-05-01
tags:
    - ReadingNotes
---

I'm trying to learn the technique of information relaxation in stochastic DP problems. This is a reading notes for the paper `Brown, D. B., & Smith, J. E. (2022). Information relaxations and duality in stochastic dynamic programs: A review and tutorial. Foundations and Trends® in Optimization, 5(3), 246-339.`

This paper can be found at [here](https://faculty.fuqua.duke.edu/~dbbrown/bio/papers/brown_smith_2021_information_relaxation_review.pdf) or [here](https://www.nowpublishers.com/article/Details/OPT-027). Other related works include: [BSS2010](https://pubsonline.informs.org/doi/abs/10.1287/opre.1090.0796?casa_token=d4SnoQvIeM8AAAAA:jY9Dc3tAilZeGiLicBVvQnFP_uyDZSMHjLUmFRkBIOwRQMWiSSSFW1NHeXye9BMmfDaaVrvqWZcVsg), [BS2014](https://pubsonline.informs.org/doi/abs/10.1287/opre.2014.1322?casa_token=o4sbMCFd40AAAAAA:eEEjTOg6TuHcCsih8t_28bZQW6RM61kUq_aemZwrAcaLGbqYZ1LzyYkUW7_aK_w_6P773gk378CrzA), and [BS2020](https://pubsonline.informs.org/doi/abs/10.1287/mnsc.2019.3342?casa_token=GL-k_aO3UeoAAAAA:sa42TtY2DZu2JrN6nbqwRAaKKTDAM-TnDBOexixCmBTaXnuDu-oBjyOmgxlBJCba2f4I78pQt2o96w).


$$\newcommand{\Acal}{\mathcal{A}}$$
$$\newcommand{\Fcal}{\mathcal{F}}$$
$$\newcommand{\Gcal}{\mathcal{G}}$$
$$\newcommand{\Pcal}{\mathcal{P}}$$
$$\newcommand{\Zcal}{\mathcal{Z}}$$
$$\newcommand{\Embb}{\mathbb{E}}$$
$$\newcommand{\Fmbb}{\mathbb{F}}$$
$$\newcommand{\Gmbb}{\mathbb{G}}$$


Information relaxation framework
===

Information relaxation is to relax the _nonaticipativity constraint_ such that a decision maker can use more information than is truly available. Analaguous to Lagrangian relaxation in linear program or convex program, the information relaxation yields a larger feasible set (of policies) and hence a lower bound for minimization and upper bound for maximization problems. However, this relaxation is implicit in the way that we don't have an exact penalty function. And without any penalty for using addtional information, the bound may be too loose. The __information relaxation and duality__ proposed by [Brown et al.](https://pubsonline.informs.org/doi/abs/10.1287/opre.1090.0796?casa_token=d4SnoQvIeM8AAAAA:jY9Dc3tAilZeGiLicBVvQnFP_uyDZSMHjLUmFRkBIOwRQMWiSSSFW1NHeXye9BMmfDaaVrvqWZcVsg) in 2010 provides a good framework to deal with that issue.

I'll remove everything about the measure theory since I'm no expert at all. Please check out the original paper for that.

Consider at period of $t$ the decision maker has a set of information $\Fcal_t$. The sequence of the set of information available at each period is denoted as $\Fmbb = (\Fcal_0, \cdots, \Fcal_T)$, and $\Fcal_t \subseteq \Fcal_{t+1} \subseteq \Fcal$. A policy is _nonanticipative_ if, at each period, the decision depends only on the available information, $\alpha \in \Acal_\Fmbb$.

Consider a stochastic dynamic program where the objective is to find a policy to minimize the expected total costs,

$$
\inf_{\alpha \in \Acal_\Fmbb} \Embb[c(\alpha)]
$$

where $c(\alpha) = \sum_t c_t(a_t)$, $a_t$ is the $t$-period action by following the policy $\alpha$, $c_t(\cdot)$ is the cost function. Such a problem is intractable sometimes due to the curse of dimensionality.

Now consider another sequence of sets of information $\Gmbb = (\Gcal_0,\cdots,\Gcal_T)$ such that $\Fcal_t \subseteq \Gcal_t \subseteq \Fcal$, which results in

$$
\Acal_\Fmbb \subseteq \Acal_\Gmbb \subseteq \Acal
$$

Then, we relax the nonanticipativity constraint if we search an optimal policy in the enlarged set $\Acal_\Gmbb$. Again, consider the Lagrangian relaxation. When one constraint is relaxed, we add a penalty associated with that constraint (which is also a function of the Lagrangian multipliers) to the original objective function. However, in the information relaxation case, there is no explicit function for us to measure how much a policy violated the constraint. Therefore, we have to choose a proper penalty function first.

Denote $\Zcal$ the set of all penalty functions, and a set of _dual feasible penalties_ is defined as

$$
\Zcal_{\Fmbb}:=\{z \in \Zcal: \Embb[z(\alpha_F)] \geq 0,\, \forall\, \alpha_F \in \Acal_\Fmbb\}
$$

This shows that if the policy is nonanticipative, then there is no penalty, but there will be positive penalty otherwise. Two important conclusions are the weak and strong duality theorems.

- Weak duality. For all $\alpha_F \in \Acal_\Fmbb$, $z(\cdot) \in \Zcal_\Fmbb$

$$
\Embb[c(\alpha_F)] \geq \inf_{\alpha_G \in \Acal_\Gmbb} \Embb[c(\alpha_G) - z(\alpha_G)]
$$

- Strong duality.

$$
\inf_{\alpha_F \in \Acal_\Fmbb}\Embb[c(\alpha_F)] = \sup_{z \in \Zcal_\Fmbb} \inf_{\alpha_G \in \Acal_\Gmbb} \Embb[c(\alpha_G) - z(\alpha_G)]
$$

For any given nonanticipative policy $\alpha_F$, we wish to know how good it performs measured by the distance between $\Embb[c(\alpha_F)]$ and $\inf_{\alpha \in \Acal_\Fmbb}\Embb[c(\alpha)]$. However, the latter may not be available. Using both weak and strong duality, we obtain 

$$
\Embb[c(\alpha_F)] \geq \inf_{\alpha \in \Acal_\Fmbb}\Embb[c(\alpha)] \geq  \inf_{\alpha_G \in \Acal_\Gmbb} \Embb[c(\alpha_G) - z(\alpha_G)]
$$

and instead, we can use the gap between the upper and lower bounds about the optimality as the measure of performance (actually, the lower bound is also called performance bound in this case). If the gap is small, then we can conclude that the given policy performs well since it is not far from the optimality. However, a larger gap may result from a bad policy or a loose lower bound. 

One might notice that the relaxed minimization problem could be as hard as the original one. The authors show that we may want to focus on a set of structured policies when that happens. If there exists a structured policy $\Pcal \subseteq \Acal$ such that the optimal policy follows this structure,

$$
\inf_{\alpha \in \Acal_\Fmbb} \Embb[c(\alpha)] = \inf_{\alpha \in \Pcal_\Fmbb} \Embb[c(\alpha)]
$$

then we can use another performance bound since

$$
\inf_{\alpha \in \Acal_\Fmbb}\Embb[c(\alpha)] \geq  \inf_{\alpha_G \in \Pcal_\Gmbb} \Embb[c(\alpha_G) - z(\alpha_G)] \geq \inf_{\alpha_G \in \Acal_\Gmbb} \Embb[c(\alpha_G) - z(\alpha_G)]
$$

However, it is critical for an optimal policy to be contained in the structured policy set. In the case it is not, one cannot guarantee a lower bound by only focusing on the structured policies.

In order to use this framework, one has to 1) choose an information relaxation $\Gmbb$ and 2) a penalty function $z(\cdot)$. Commonly used information relaxation includes _perfect information relaxation_ such that all random variables are realized before a single decision is made. A good penalty function could be found by

$$
z_t(a) = \Embb[w_t(a) \:\vert\: \Gcal_T] - \Embb[w_t(a) \:\vert\: \Fcal_T]
$$

which exactly measures the benefits of knowing the additional information. Commonly used generating function can be instant cost plus the approximation of the value function for the future.

Applications
===

- Inventory management.


The goal is to find a policy for ordering a single product over $T+1$ periods ($t=0,1,\cdots,T$) to minimize the expected total costs, which consists of ordering costs, holding costs, and backordering costs. The inventory level is $x_t$, the demand $d_t$ is random variable with known distribution. It is realized after the ordered amount $a_t$ is determined at $t$-period. The inventory level evolves according to 

$$ x_{t+1} = x_t + a_t - d_t,\quad x_0 = 0 $$

Cost functions are linear: order cost is $k_t a_t$, holding cost is $h_t[x_{t+1}]^+$, and backordering cost is $-p_t [x_{t+1}]^-$. The objective is

$$
\begin{align*}
\inf_{a_0,\cdots,a_T}\;\; & \mathbb{E}\left[\sum_{t=0}^T k_t a_t + \max\{h_t x_{t+1}, -p_t x_{t+1}\}\right] \\
\text{s.t.}\;\; & x_{t+1} = x_t + a_t - d_t \\
& x_0 = 0 \\
& a_t \geq 0, \quad \forall\, t \\
\end{align*}
$$

For more discussion and simulation, see this [ipynb file]({{ site.url }}/ipynb/information_relaxation/inventory_management.html).

- Dynamic Assortment Planning.

todo.

- Portfolio Optimization with Transaction Costs.

todo.