
- [Azure Monitor Documentation](https://docs.microsoft.com/en-us/azure/azure-monitor/)
- [Azure Application Insights Documentation](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)
- [Azure Log Analytics Documentation](https://docs.microsoft.com/en-us/azure/azure-monitor/logs/log-analytics-overview)
## Application Insight

Application Insight is a free service (costs if you want extra features) that extends within Azure Monitor in Azure and provides your application/service with essential information about call rates, diagnostic problems, critical errors or see real-time exceptions within your application and what caused them via the Azure Portal interface. Application Insight gathers critical information that you can utilize to get a better understanding of how your app performs in all kind of ways.

Application Insight offers 
-   Exceptions and performance diagnostics.
-   Interactive data analysis.
-   Azure diagnostics.
-   Proactive detection.
---
## Metrics & Log data
#### Azure Monitor
Azure Monitor is a cloud-based service that provides real-time monitoring and diagnostics for Azure resources and applications. The core concept of using Azure Monitor is to gain insight into the performance, availability, and health of your resources and applications, and to identify and resolve issues that may arise.

-   Azure Monitor collects metrics and logs from Azure resources and applications.
-   It can be configured to send alerts when specific conditions are met.
-   It provides tools for visualizing and analyzing data.
-   It helps with understanding resource performance, identifying issues, and troubleshooting.
#### Scaling azure resources

| Scaling Feature       | Description | Real-World Scenario Example |
|-----------------------|-------------|-----------------------------|
| `Default Profile`     | Always active, except when overridden by specific profiles. | Ideal for general baseline scaling when no special events or predictable patterns are expected. |
| `Recurring Profiles`  | Activated during specific times on selected days, repeating weekly. | Useful for businesses with predictable high-traffic periods, like weekends for retail websites. |
| `Fixed Date and Time Profiles` | Active during specific time ranges on specific dates. | Perfect for one-time events, like online sales or marketing campaigns, requiring additional resources. |
| `Predictive Autoscale` | Uses machine learning to forecast and scale out before workload spikes. | Suitable for workloads with cyclical patterns, like a streaming service experiencing higher viewership during certain hours. |
#### Types of alerts

| Alert type | Description |
| ---- | ---- |
| Metric | Metric alerts evaluate resource metrics at regular intervals. Metrics can be platform metrics, custom metrics, logs from Azure Monitor converted to metrics, or Application Insights metrics. Metric alerts can also apply multiple conditions and dynamic thresholds. |
| Log alerts | Log alerts allow users to use a Log Analytics query to evaluate resource logs at a predefined frequency. |
| Activity Log | Activity log alerts are triggered when a new activity log event occurs that matches defined conditions. Resource Health alerts and Service Health alerts are activity log alerts that report on your service and resource health. |
| Smart detection | Smart detection on an Application Insights resource automatically warns you of potential performance problems and failure anomalies in your web application. You can migrate smart detection on your Application Insights resource to create alert rules for the different smart detection modules. |

#### Dynamic thresholds for Metric alerts
We recommend configuring alert rules with dynamic thresholds on these metrics:
- Virtual machine CPU percentage
- Application Insights HTTP request execution time

**Best practise when configuring dyanmic thresholds**
1.  In the **Thresholds** field, select **Dynamic**.
2. In the **Aggregation type**, we recommend that you don't select **Maximum**.
3. In the **Operator** field, select **Greater than** unless behavior represents the application usage.
4. In **Threshold Sensitivity**, select **Medium** or **Low** to reduce alert noise.
5. In the **Check every** field, consider lowering the frequency based on the business impact of the alert.
6. In the **Lookback period**, set the look-back window to at least 15 minutes. For example, if the **check every** field is set to 5 minutes, the lookback period should be at least 3 minutes or more.
