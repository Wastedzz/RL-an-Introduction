# Finite Markov Decision Processes

## Some Important Notations in This Chapter
### Four arguments dynamics of the MDP:
$$
p(s',r\mid s,a)=\mathrm{Pr}\{S_t=s',R_t=r\mid S_{t-1}=s,A_t=a \} \tag{1.1}
$$
This is the core equation of this chapter.
### Some probability relationships between dynamics of MDP
$$\begin{aligned}
   p(s'\mid s,a) &= \sum_{r\in\mathbb{R}}p(s',a\mid s,a)  \tag{1.2}
\end{aligned} $$
$$\begin{aligned}
   r(s,a) &= \mathbb{E}[R_t\mid S_{t-1=}s,A_{t-1}=a]=\sum_{r\in\mathbb{R}}r
   p(r\mid s,a) \\&= \sum_{r\in\mathbb{R},s'\in\mathcal{S}}rp(s',r\mid s,a) \tag{1.3}
\end{aligned}$$
$$\begin{aligned}
   r(s',s,a) &= \mathbb{E}\{r\mid s',s,a\}=\sum_{r\in\mathbb{R}}rp(r\mid s',s,a)\\ &=\sum_{r\in\mathbb{R}}r\frac{p(s',r\mid s,a)}{p(s'\mid s,a)} \tag{1.4}
\end{aligned}$$
$$\begin{aligned}
    G_t=\sum_{k=0}^{\infty}\gamma^{k}R_{t+k+1}=R_{t+1}+\gamma G_{t+1} \tag{1.5}
\end{aligned}$$

### State-value and state-action function
$$
\begin{aligned}
v_{\pi}(s) &=\mathbb{E}[G_t\mid S_t=s] =\mathbb{E}[\sum_{k=0}^{\infty}\gamma^{k}R_{t+k+1}\mid S_t=s] \tag{1.6}
\end{aligned}
$$
$$
\begin{aligned}
q_{\pi}(s,a) &=\mathbb{E}[G_t\mid S_t=s,A_t=a] \\ &=\mathbb{E}[\sum_{k=0}^{\infty}\gamma^{k}R_{t+k+1}\mid S_t=s,A_t=a]   \tag{1.7}
\end{aligned}
$$
### Recursive formula of state-value and state-action function
Due to **Markov Property**, the most important step in the derivation as follows:
$$
\begin{aligned}
    \mathbb{E}[G_{t+1}\mid S_t=s] &= \sum_{s'\in\mathcal{S}}p(s'\mid s)\mathbb{E}[G_{t+1}\mid S_{t+1}=s',S_t=s] \\
    &=\sum_{s'\in\mathcal{S},a\in\mathcal{A}}\pi(a\mid s)p(s'\mid s,a)\mathbb{E}[G_{t+1}\mid S_{t+1}=s']\\ &=\sum_{s'\in\mathcal{S},a\in\mathcal{A},r\in\mathcal{R}}\pi(a\mid s)p(s',r\mid s,a)v_{\pi}(s')
\end{aligned}
$$
$$
\begin{aligned}
    \mathbb{E}[G_{t+1}\mid S_t=s,A_t=a] &= \sum_{s'\in\mathcal{S}}p(s'\mid s,a)\mathbb{E}[G_{t+1}\mid S_{t+1}=s',S_t=s,A_t=a] \\
    &=\sum_{s'\in\mathcal{S}}p(s'\mid s,a)\mathbb{E}[G_{t+1}\mid S_{t+1}=s']\\ &=\sum_{s'\in\mathcal{S}}p(s'\mid s,a)\sum_{a'\in\mathbb{A}}\pi(a'\mid s')\mathbb{E}[G_{t+1}\mid S_{t+1}=s',A_{t+1}=a']\\ &=\sum_{s'\in\mathcal{S},r\in\mathcal{R}}p(s',r\mid s,a)\sum_{a'\in\mathbb{A}}\pi(a'\mid s')q_{\pi}(s',a') 
\end{aligned} 
$$
Among these equations,we make full use of **Total Probability Rule**.Take above equations and $(1.5)$ into $(1.6)$ and $(1.7)$,for all $s\in\mathcal{S},a\in\mathcal{A}$,we obtain:
$$
\begin{aligned}
    v_{\pi}(s) &=\sum_{s'\in\mathcal{S},a\in\mathcal{A},r\in\mathcal{R}}\pi(a\mid s)p(s',r\mid s,a)(r+\gamma v_{\pi}(s')) \tag{1.8}
\end{aligned}
$$
$$
\begin{aligned}
    q_{\pi}(s,a) &=\sum_{s'\in\mathcal{S},r\in\mathcal{R}}p(s',r\mid s,a)(r+\gamma\sum_{a'\in\mathbb{A}}\pi(a'\mid s')q_{\pi}(s',a'))  \tag{1.9}
\end{aligned}
$$
From$(1.8)$ and $(1.9)$,we can get the state and state-action value function under deterministic strategy with following fomula with initial $v_{\pi}^{0}(s)$ and $q_{\pi}^{0}(s,a)$: for all $s\in\mathcal{S},a\in\mathcal{A}$
$$
\begin{aligned}
    v_{\pi}^{n+1}(s) &=\sum_{s'\in\mathcal{S},a\in\mathcal{A},r\in\mathcal{R}}\pi(a\mid s)p(s',r\mid s,a)(r+\gamma v_{\pi}^{n}(s')) \tag{1.10}
\end{aligned}
$$
$$
\begin{aligned}
    q_{\pi}^{n+1}(s,a) &=\sum_{s'\in\mathcal{S},r\in\mathcal{R}}p(s',r\mid s,a)(r+\gamma\sum_{a'\in\mathbb{A}}\pi(a'\mid s')q_{\pi}^{n}(s',a'))  \tag{1.11}
\end{aligned}
$$
As we want to get a strategy in a MDP problem,that is,when we are in a states,which action shall we choose can make the largest long-term reward,we can select action as followed:$s\in\mathcal{S},a\in\mathcal{A}$:
$$
\begin{aligned}
    v_{\pi}(s) &=\max_{a\in\mathcal{A}}\sum_{s'\in\mathcal{S},r\in\mathcal{R}}p(s',r\mid s,a)(r+\gamma v_{\pi}(s')) \tag{1.12}
\end{aligned}
$$
$$
\begin{aligned}
    q_{\pi}(s,a) &=\sum_{s'\in\mathcal{S},r\in\mathcal{R}}p(s',r\mid s,a)(r+\gamma\max_{a'\in\mathcal{A}}q_{\pi}(s',a'))  \tag{1.13}
\end{aligned}
$$
Intuitively, optimal value: $v^*(s)$ and $q^*(s,a)$ also confirm $(1.12)$ and $(1.13)$,which called ***Bellman optimality equation*** . Actually, we can have an intuition that $q^*(s,a)$ is superior to $v^*(s)$ during we get the optimal strategy according to  the position of the $max$ operator in $(1.12)$ and $(1.13)$.

### Realtions between $v(s)$ and $q(s,a)$
&ensp;&ensp;
Also by means of **Markov Property** and **Total Probability Rule**, we can derive some relationships as followed:
$$
\begin{aligned}
    v_{\pi}(s) &=\sum_{a\in\mathcal{A}}\pi(a\mid s)q_{\pi}(s,a)  \tag{1.14}
\end{aligned}
$$
$$
\begin{aligned}
    q_{\pi}(s,a) &=\sum_{s'\in\mathcal{S},r\in\mathcal{R}}p(s',r\mid s,a)(r+\gamma v_{\pi}(s'))  \tag{1.15}
\end{aligned}
$$






