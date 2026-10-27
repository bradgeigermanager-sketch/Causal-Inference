Here are two detailed causal inference scenarios analyzed through both Pearl’s Structural Causal Models (SCMs) and Rubin’s Potential Outcomes Framework.

Scenario 1: Digital Marketing & Customer Retention

1. Scenario Title: E-Commerce Loyalty Program Impact on Churn

2. Core Variables:
*   Treatment (X): Enrollment in the "Premium Loyalty Program" (Binary: 1 = Enrolled, 0 = Not Enrolled).
*   Outcome (Y): Customer Churn Rate within 6 months (Binary: 1 = Churned, 0 = Retained). Note: We aim to minimize Y.
*   Confounder (Z): Historical Purchase Frequency (Continuous: Average orders per month prior to intervention). High-frequency buyers are more likely to be invited to the program *and* less likely to churn naturally.

3. Causal Mechanism & ASCII DAG:
Historical Purchase Frequency ($Z$) drives both the likelihood of being selected for the loyalty program ($X$) and the inherent loyalty/retention probability ($Y$). The program itself ($X$) also directly influences retention ($Y$).

```text
      Z (Purchase Frequency)
     / \
    v   v
X (Program) --> Y (Churn)
```

4. Pearl's SCM Approach (Graph-Driven Blocking):

*   Step 1: Identify Target: We want to estimate the Average Treatment Effect (ATE) of the program on churn: $P(Y | do(X=1)) - P(Y | do(X=0))$.
*   Step 2: Backdoor Verification: The path $X \leftarrow Z \rightarrow Y$ is a backdoor path from $X$ to $Y$. Since $Z$ is observed, we can block this non-causal association by conditioning on $Z$. There are no colliders or unobserved confounders in this simplified model.
*   Step 3: Adjustment Expression:
    $$ P(Y | do(X=x)) = \sum_{z} P(Y | X=x, Z=z) P(Z=z) $$
*   Step 4: The Mechanism: Practically, this means we stratify customers by their historical purchase frequency. We calculate the churn rate difference between enrolled and non-enrolled users *within* each frequency bracket, then average these differences weighted by how common each frequency bracket is in the total population. This removes the bias that "high-frequency users churn less anyway."

5. Rubin's Potential Outcomes Approach (Data Balancing):

*   Step 1: Define Potential Outcomes: Let $Y_i(1)$ be the churn status of customer $i$ if enrolled, and $Y_i(0)$ if not. The individual treatment effect is $\tau_i = Y_i(0) - Y_i(1)$ (since lower churn is better).
*   Step 2: Propensity Score Calculation: We model the probability of enrollment given history: $e(z) = P(X=1 | Z=z)$. Typically estimated via logistic regression where $X$ is the outcome and $Z$ is the predictor.
*   Step 3: Weight Application (IPW): We assign weights to create a pseudo-population where treatment assignment is independent of $Z$.
    $$ w_i = \frac{X_i}{e(Z_i)} + \frac{1-X_i}{1-e(Z_i)} $$
*   Step 4: The Mechanism: By weighting observations inversely to their probability of receiving the treatment they actually received, we up-weight rare cases (e.g., low-frequency buyers who got into the program) and down-weight common cases. This creates a synthetic dataset where the distribution of Purchase Frequency ($Z$) is identical between the Enrolled and Non-Enrolled groups, allowing a simple difference in means to estimate the causal effect.

---

Scenario 2: Healthcare & Drug Efficacy

1. Scenario Title: New Cardiac Medication Impact on Recovery Time

2. Core Variables:
*   Treatment (X): Administration of New Drug B (Binary: 1 = Received Drug B, 0 = Standard Care).
*   Outcome (Y): Days until stable discharge (Continuous). Lower is better.
*   Confounder (Z): Severity of Initial Condition (Ordinal: Mild, Moderate, Severe). Doctors are more likely to prescribe the new experimental drug to severe cases (compassionate use or trial criteria), and severe cases naturally take longer to recover.

3. Causal Mechanism & ASCII DAG:
Severity ($Z$) influences the doctor's decision to prescribe Drug B ($X$) and directly impacts recovery time ($Y$). Drug B ($X$) also directly impacts recovery time ($Y$).

```text
      Z (Severity)
     / \
    v   v
X (Drug B) --> Y (Recovery Days)
```

4. Pearl's SCM Approach (Graph-Driven Blocking):

*   Step 1: Identify Target: Estimate the expected recovery time under universal adoption vs. standard care: $E[Y | do(X=1)] - E[Y | do(X=0)]$.
*   Step 2: Backdoor Verification: The backdoor path $X \leftarrow Z \rightarrow Y$ introduces bias. Conditioning on $Z$ (Severity) blocks this path. We assume no unmeasured confounders (e.g., genetic factors affecting both prescription eligibility and recovery are absent or captured in severity).
*   Step 3: Adjustment Expression:
    $$ E[Y | do(X=x)] = \sum_{z} E[Y | X=x, Z=z] P(Z=z) $$
*   Step 4: The Mechanism: In operational terms, we compare recovery times for Drug B vs. Standard Care separately for Mild, Moderate, and Severe patients. If Drug B works better for Severe patients but worse for Mild ones, this method captures those heterogeneous effects. We then aggregate these stratum-specific effects based on the overall prevalence of severity levels in the hospital population.

5. Rubin's Potential Outcomes Approach (Data Balancing):

*   Step 1: Define Potential Outcomes: $Y_i(1)$ is days to discharge if patient $i$ takes Drug B; $Y_i(0)$ is days if they take Standard Care.
*   Step 2: Propensity Score Calculation: Estimate $e(z) = P(X=1 | Z=z)$. Given $Z$ is ordinal, this might involve dummy variables or a generalized linear model predicting drug assignment based on severity indicators.
*   Step 3: Weight Application (IPW):
    $$ \hat{ATE} = \frac{1}{N} \sum_{i=1}^{N} \left[ \frac{X_i Y_i}{e(Z_i)} - \frac{(1-X_i) Y_i}{1-e(Z_i)} \right] $$
*   Step 4: The Mechanism: The weights adjust for the fact that Severe patients are over-represented in the Drug B group. A Severe patient on Drug B gets a lower weight (because they were likely to get it), while a Mild patient on Drug B gets a higher weight (because they were unlikely to get it). This rebalances the sample so that the "Drug B" group looks like a random sample of all severity levels, enabling an unbiased comparison of mean recovery days.

* [Generate another set of random-domain scenarios](prompt:continue with another random-domain selection and discussion using the same framework layout)
* [Pivot to a code implementation of one of these scenarios](prompt:write a python simulation generating data for one of these scenarios and calculate the ATE using both IPW and g-computation to prove they match)<br><br>百度AI生成，内容仅供参考