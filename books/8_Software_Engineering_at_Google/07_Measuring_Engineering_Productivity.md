### 7. Measuring Engineering Productivity

* Measuring productivity is expensive, so first of all it needs to be made sure that it is worth the effort for the specific project: Would the stakeholders do anything differently based on the measurement results? Would the analysis mechanism be convincing to the decision makers?
* Lines of written code is obviously a bad productivity metric. It should rather be seen as "lines of code *spent*"
* Google uses the *GSM* framework for metrics: *Goals* / *Signals* / *Metrics*
    * Goal: What is the thing that you want to understand at a high level? This should not say anything about how you want to measure it
    * Signal: Things you would like to measure, but that are not necessarily directly measurable. This can also be more high level than metrics
    * Metric: Something measurable that might be a proxy for the signal. There might be multiple metrics for a given signal
* Advantages of GSM
    * The GSM framework prevents the *streetlight effect*: Only looking for your lost keys under the streetlight, when in fact you should also be looking at places where you cannot see that easily
    * GSM also prevents us from measuring metrics that are not important for any end goal
    * It is easy to see coverage, i.e. understand what goals you have metrics for
* Google’s goals for engineering productivity, which are traded off against each other: QUANTS
    1. **Q**uality of code
    2. **A**ttention from engineers: How often do engineers reach a state of flow, and how often do they get distracted by notifications?
    3. **I**ntellectual complexity: How much complexity is involved in making something work?
    4. **T**empo and velocity: How quickly do engineers accomplish their tasks?
    5. **S**atisfaction: How satisfied are engineers with their tools and end product?
* For each measurement project (e.g. readability), the engineering productvity team tries to determine more specific QUANTS goals
* *Recall bias*: People are more likely to recall particularly interesting or unusual events
* Example GSM for readability:
    * Goal: Engineers write higher-quality code
    * Signal: Engineers who have granted readability find their code to be of higher quality than those who do not have readability
    * Metric: Surveys send out to said engineers
* Having one central team evaluate engineering productivity brings benefits to the entire company
