
## Bayes’ Theorem

$$ P(A|B) = \frac{P(A)P(B|A)} {P(B)} $$

Probability theory is the mathematical framework for quantifying uncertainty and randomness. It underpins a wide range of fields, including statistics, finance industry, engineering, and the natural sciences. Stochastic processes extend probability theory to model systems that evolve over time under uncertainty.

## Fundamental Concepts in Probability

1. Sample Space and Events
   - **Sample Space (Ω)**: The set of all possible outcomes.
   - **Event (A, B, ...)**: A subset of the sample space.

2. Probability Axioms
   - **Non-negativity**: \( P(A) \geq 0 \) for any event \( A \).
   - **Normalization**: \( P(\Omega) = 1 \).
   - **Additivity**: For mutually exclusive events \( A \) and \( B \),
     \[ P(A \cup B) = P(A) + P(B) \].

3. **Conditional Probability**
   - **Definition**: The probability of \( A \) given \( B \):
     \[ P(A|B) = \frac{P(A \cap B)}{P(B)}, \quad \text{if } P(B) > 0 \].

4. **Bayes' Theorem**
   - **Formula**:
     \[ P(A|B) = \frac{P(B|A) P(A)}{P(B)} \].
   - **Total Probability Theorem**:
     \[ P(B) = \sum_{i} P(B|A_i) P(A_i) \], where \( \{A_i\} \) is a partition of \( \Omega \).

5. **Independence**
   - **Events** \( A \) and \( B \) are independent if:
     \[ P(A \cap B) = P(A) P(B) \].

## Random Variables and Distributions

1. **Random Variables**
   - **Discrete Random Variable**: Takes countable values.
   - **Continuous Random Variable**: Takes values in a continuum.

2. **Probability Functions**
   - **Probability Mass Function (PMF)** for discrete variables:
     \[ p_X(x) = P(X = x) \].
   - **Probability Density Function (PDF)** for continuous variables:
     \[ f_X(x) \], where \( P(a \leq X \leq b) = \int_{a}^{b} f_X(x) dx \).
   - **Cumulative Distribution Function (CDF)**:
     \[ F_X(x) = P(X \leq x) \].

3. **Expected Value and Moments**
   - **Expected Value (Mean)**:
     - Discrete:
       \[ E[X] = \sum_{x} x p_X(x) \].
     - Continuous:
       \[ E[X] = \int_{-\infty}^{\infty} x f_X(x) dx \].
   - **Variance**:
     \[ \text{Var}(X) = E[(X - E[X])^2] = E[X^2] - (E[X])^2 \].
   - **Standard Deviation**:
     \[ \sigma_X = \sqrt{\text{Var}(X)} \].

4. **Common Distributions**
   - **Binomial Distribution**:
     \[ P(X = k) = \binom{n}{k} p^k (1-p)^{n-k} \], \( k = 0,1,\dots,n \).
   - **Poisson Distribution**:
     \[ P(X = k) = \frac{\lambda^k e^{-\lambda}}{k!} \], \( k = 0,1,2,\dots \).
   - **Normal (Gaussian) Distribution**:
     \[ f_X(x) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{(x - \mu)^2}{2\sigma^2}} \].
   - **Exponential Distribution**:
     \[ f_X(x) = \lambda e^{-\lambda x} \], \( x \geq 0 \).
   - **Uniform Distribution**:
     \[ f_X(x) = \frac{1}{b - a} \], \( x \in [a, b] \).

---

## Joint Distributions and Independence

1. **Joint PMF/PDF**
   - For two random variables \( X \) and \( Y \):
     - **Joint PMF** (discrete):
       \[ p_{X,Y}(x, y) = P(X = x, Y = y) \].
     - **Joint PDF** (continuous):
       \[ f_{X,Y}(x, y) \].

2. **Marginal Distributions**
   - **Discrete**:
     \[ p_X(x) = \sum_{y} p_{X,Y}(x, y) \].
   - **Continuous**:
     \[ f_X(x) = \int_{-\infty}^{\infty} f_{X,Y}(x, y) dy \].

3. **Covariance and Correlation**
   - **Covariance**:
     \[ \text{Cov}(X, Y) = E[(X - E[X])(Y - E[Y])] = E[XY] - E[X]E[Y] \].
   - **Correlation Coefficient**:
     \[ \rho_{X,Y} = \frac{\text{Cov}(X, Y)}{\sigma_X \sigma_Y} \], where \( \sigma_X \) and \( \sigma_Y \) are standard deviations.

4. **Independence of Random Variables**
   - \( X \) and \( Y \) are independent if:
     \[ P(X \leq x, Y \leq y) = P(X \leq x) P(Y \leq y) \].
   - For PDFs:
     \[ f_{X,Y}(x, y) = f_X(x) f_Y(y) \].

---

## Important Theorems and Inequalities

1. **Law of Large Numbers**
   - **Weak Law**: Sample averages converge in probability to the expected value as sample size increases.
     \[ \lim_{n \to \infty} P\left( \left| \bar{X}_n - \mu \right| \geq \varepsilon \right) = 0 \].
   - **Strong Law**: Sample averages converge almost surely to the expected value.

2. **Central Limit Theorem (CLT)**
   - The sum (or average) of a large number of independent, identically distributed (i.i.d.) random variables with finite variance tends toward a normal distribution.

3. **Chebyshev's Inequality**
   - For any \( k > 0 \):
     \[ P(|X - E[X]| \geq k \sigma) \leq \frac{1}{k^2} \].

4. **Markov's Inequality**
   - For a non-negative random variable \( X \) and \( a > 0 \):
     \[ P(X \geq a) \leq \frac{E[X]}{a} \].

5. **Jensen's Inequality**
   - For a convex function \( \phi \) and random variable \( X \):
     \[ \phi(E[X]) \leq E[\phi(X)] \].

---

## Moment Generating Functions (MGFs)

1. **Definition**
   - The MGF of \( X \):
     \[ M_X(t) = E[e^{tX}] \].

2. **Properties**
   - MGFs uniquely determine the distribution if they exist in an open interval around \( t = 0 \).
   - Moments can be found by differentiating the MGF:
     \[ E[X^n] = M_X^{(n)}(0) \], where \( M_X^{(n)}(0) \) is the \( n \)-th derivative evaluated at \( t = 0 \).

---

## Stochastic Processes

1. **Definition**
   - A collection of random variables \( \{X(t) : t \in T\} \) indexed by time \( t \).

2. **Types of Stochastic Processes**
   - **Discrete-Time Processes**: Time parameter \( t \) takes discrete values.
   - **Continuous-Time Processes**: Time parameter \( t \) is continuous.

3. **Markov Processes**
   - **Definition**: The future state depends only on the current state, not on the past states (memoryless).
   - **Transition Probability**:
     \[ P_{ij} = P(X_{n+1} = j | X_n = i) \].

4. **Chapman-Kolmogorov Equations**
   - For Markov processes:
     \[ P_{ij}^{(n+m)} = \sum_{k} P_{ik}^{(n)} P_{kj}^{(m)} \].

5. **Stationary Distribution**
   - A probability distribution \( \pi \) satisfying:
     \[ \pi = \pi P \], where \( P \) is the transition matrix.

---

## Specific Stochastic Processes

1. **Poisson Process**
   - **Definition**: Counts the number of events in a fixed interval of time or space.
   - **Properties**:
     - **Increment Independence**: Non-overlapping increments are independent.
     - **Increment Stationarity**: The distribution of the number of events depends only on the length of the interval.
   - **Probability of \( k \) Events in Time \( t \)**:
     \[ P(N(t) = k) = \frac{(\lambda t)^k e^{-\lambda t}}{k!} \].

2. **Brownian Motion (Wiener Process)**
   - **Definition**: A continuous-time stochastic process with continuous paths.
   - **Properties**:
     - \( W(0) = 0 \).
     - **Independent Increments**: \( W(t) - W(s) \) is independent of the past, for \( t > s \).
     - **Normal Distribution of Increments**:
       \[ W(t) - W(s) \sim N(0, t - s) \].

3. **Martingales**
   - **Definition**: A process \( \{X_n\} \) where the conditional expectation of the next value, given all prior values, equals the present value:
     \[ E[X_{n+1} | X_1, X_2, \dots, X_n] = X_n \].

---

## Itô's Lemma (Stochastic Calculus)

- **Formula**:
  If \( X(t) \) follows a stochastic differential equation and \( f(t, X(t)) \) is a twice-differentiable function, then:
  \[ df = \left( \frac{\partial f}{\partial t} + \mu \frac{\partial f}{\partial x} + \frac{1}{2} \sigma^2 \frac{\partial^2 f}{\partial x^2} \right) dt + \sigma \frac{\partial f}{\partial x} dW(t) \],
  where \( \mu \) and \( \sigma \) are drift and diffusion coefficients, and \( W(t) \) is Brownian motion.

---

## Additional Important Formulas

1. **Law of Total Expectation**
   - \( E[X] = E[E[X|Y]] \).

2. **Variance Decomposition**
   - \( \text{Var}(X) = E[\text{Var}(X|Y)] + \text{Var}(E[X|Y]) \).

3. **Characteristic Function**
   - \( \phi_X(t) = E[e^{itX}] \).
   - **Relation to Distribution**: The characteristic function uniquely determines the probability distribution.

4. **Convergence in Distribution**
   - A sequence of random variables \( X_n \) converges in distribution to \( X \) if:
     \[ \lim_{n \to \infty} P(X_n \leq x) = P(X \leq x) \], for all \( x \) where \( P(X \leq x) \) is continuous.

---
