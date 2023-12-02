<img src="/insight_demo.png">
<h2>Background/Pain-point</h2>

<table>
  <tr>
   <td colspan="3" >
<ol>

<li>The tracking data quality is low, mainly due to bugs during app/web release.

<li>The issue can be detected late and cause big data loss.

<li>Current solution: tracking volume alert is sent but have some limitations: 
<ol>
 
<li>Many alerts sent + High false alert rate > Hard for PIC to check all alerts.
 
<li>No specific feedback/action required for the alert.
 
<li>The flow to subscribe for alerts is not clear.
 
<li>No visualization for alert.
</li> 
</ol>
</li> 
</ol>
   </td>
  </tr>
  <tr>
   <td colspan="3" >There are 3 main directions/method to improve tracking quality
   </td>
  </tr>
  <tr>
   <td><strong>Method</strong>
   </td>
   <td><strong>Advantage</strong>
   </td>
   <td><strong>Disadvantage</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Prevention Method</strong>
<p>
Manual QA testing: QA before app/web release 
   </td>
   <td>Prevent the issue from happening > <strong>no data loss </strong>
   </td>
   <td>Manual effort, <strong>can not scale up</strong>, except if having automated test
   </td>
  </tr>
  <tr>
   <td>Alert/Insight: System detects unusual changes or emerging trends in data and notifies PIC
   </td>
   <td>
<ol>

<li>Can apply ML to detect abnormality, <strong>can scale up </strong>to all tracking point, metrics and breakdown dimension

<li>The issue can be detected early.
</li>
</ol>
   </td>
   <td>
<ol>

<li>Tracking issue already happened, <strong>data loss </strong>(need to wait for the fix)

<li>Subject to <strong>false alert/ depend on accuracy level</strong>
</li>
</ol>
   </td>
  </tr>
  <tr>
   <td>Tracking issue SOP: Users detect unusual changes or emerging trends
   </td>
   <td>
<ol>

<li>Quite accurate to detect the issue 

<li>Can breakdown by any dimension to figure out the root cause 
</li>
</ol>
   </td>
   <td>
<ol>

<li>Tracking issue already happened, <strong>data loss </strong>(need to wait for the fix)

<li>The issue can be detected <strong>late</strong>

<li>Can <strong>take a long time to debug</strong> the issue 
</li>
</ol>
   </td>
  </tr>
  <tr>
   <td colspan="3" ><strong>All methods can be done together to have high coverage and reduce tracking issue</strong>
<p>
<strong>This PRD focuses on Alert/Insight.</strong>
   </td>
  </tr>
</table>


<h2>Solution Overview / Key Outcome </h2>



<table>
  <tr>
   <td><strong>Solution Overview</strong>
   </td>
   <td>Analytics Insight: 
<ol>

<li><strong>Apply a better time-series model</strong> to accurately detect tracking anomalies in (Daily, Weekly, Monthly and Hourly) frequency.

<li><strong>Feedback for the alert</strong>: marked alert as helpful or report issue (create JIRA ticket) to tracking point owners > feedback to improve model performance

<li><strong>Mature alert channel:</strong> users can choose metrics to monitor and send alerts to email, MM, Seatalk.
</li>
</ol>
   </td>
  </tr>
  <tr>
   <td><strong>Key Outcome </strong>
   </td>
   <td>Success Metrics: Achieve Precision > 80% while keeping Recall to be at least 50%. 
<ol>

<li>Precision: <em>How many of our alerts are actually tracking issues. </em>Defining tracking issue = number of alert that is marked as Helpful or Users create bug ticket to follow up > <strong>Target 80% of alert are marked as Helpful</strong>

<li>Recall: <em>How many of the total tracking issues were we able to identify correctly as alert?</em> --> Will keep <strong>Minimum = 50%)</strong>

<li><strong>Trade-off:</strong> 
<ol>
 
<li>if we tried to increase Precision to 100% > we will only send alerts for obvious anomalies (extreme cases) > very small number of alerts. Therefore, miss out a lot of abnormal cases --> small impact.
 
<li>If we tried to detect all issues > we will send a large number of false alerts. Therefore, PIC can not respond to all alerts.
</li> 
</ol>
</li> 
</ol>
   </td>
  </tr>
</table>


<h2>Product Roadmap: Initial Phase VS Later Phase</h2>




* Later phase could be multiple rounds of release, depending on learning from previous phase

<table>
  <tr>
   <td>
Features
   </td>
   <td><strong>Initial Phase</strong>
   </td>
   <td><strong>Later Phase</strong>
   </td>
  </tr>
  <tr>
   <td>Monitor Metrics for abnormality 
   </td>
   <td><strong>Milestone 1</strong>: Fix metrics for page_type, page_section, point, target_type, target_type_ext
<p>
User create
<ol>

<li>Record Count

<li>User Count

<li>Count Per User

<li>CTR Record Count 

<li>CTR User Count

<p>
we Default 
<ol>

<li>AU (Active User) 

<li>AD (Active Device)

<li>Users Sign up Success 

<li>Users Login Success

<li>Users View Product Page

<li>Users Add to Cart Success

<li>Users Checkout Success
</li>
</ol>
</li>
</ol>
   </td>
   <td><strong>Milestone 2</strong>: Allow users to create custom metrics by SQL
   </td>
  </tr>
  <tr>
   <td>Possible Explanation for metrics abnormality 
   </td>
   <td><strong>Milestone 1</strong>
<p>
Fix dimension breakdown = Possible explanation
<ol>

<li>Platform

<li>Region 

<li>App version

<li>RN version

<li>Compared to view of the same page_type

<li>Compared to impression/click of the same page_type, page_section, point, target_type, target_type_ext
</li>
</ol>
   </td>
   <td><strong>Milestone 2:</strong>
<ol>

<li>Allow users to select breakdown by more dimensions 
<ol>
 
<li>Device/Model
 
<li>E.g. target_property fields = is_ads
</li> 
</ol>

<li>Can provide a possible explanation by dimension combination. E.g. number of users is low for <strong>"android and app version 2.89"</strong> 
<ol>
 
<li>Reference:  
<ol>
  
<li>GA: <a href="https://support.google.com/analytics/answer/9517187?hl=en&ref_topic=10333392">https://support.google.com/analytics/answer/9517187?hl=en&ref_topic=10333392</a>
  
<li>How to apply PCA: <a href="https://visualstudiomagazine.com/articles/2021/10/20/anomaly-detection-pca.aspx">https://visualstudiomagazine.com/articles/2021/10/20/anomaly-detection-pca.aspx</a>
</li>  
</ol>
 
<li>Based on all possible dimension values, create multiple segments, then normalize each metric by the number of users within a segment. Next, run the <a href="https://en.wikipedia.org/wiki/Principal_component_analysis">PCA</a> for those segments and normalized metrics. If any particular segment demonstrates anomalous behavior across any metric and comprises at least 0.05% of the users on that property, surface those segments as anomalies.
</li> 
</ol>
</li> 
</ol>
   </td>
  </tr>
  <tr>
   <td>Alert Channel
   </td>
   <td><strong>Milestone 1</strong>
<ol>

<li>Support users to create and send alert to Seatalk, Mattermost, Email (default)

<li>Only trigger alerts for tracking point that users create alert, won't trigger alerts for ALL tracking points > Save resources
</li>
</ol>
   </td>
   <td>
   </td>
  </tr>
</table>


<h2>Modules</h2>



<table>
  <tr>
   <td><strong>#</strong>
   </td>
   <td><strong>Dev Component</strong>
   </td>
   <td><strong>Module</strong>
   </td>
   <td><strong>Background</strong>
   </td>
   <td><strong>Solution</strong>
   </td>
   <td><strong>Objective</strong>
   </td>
   <td><strong>Importance</strong>
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>Data
   </td>
   <td>ML Model
   </td>
   <td>Apply better ML to accurately detect tracking anomaly, frequency (Daily, Weekly, Monthly and Hourly) 
   </td>
   <td>
<ol>

<li><strong>Objective</strong>: to find outliers (greater mass of data serves primarily to define the "normal" against which anomalies are measured)

<li><strong>ML: </strong>Apply time-series model<strong>,</strong> account for holiday effect (e.g weekend, campaign date)
</li>
</ol>
   </td>
   <td>Achieve <a href="https://en.wikipedia.org/wiki/F-score">Precision</a> > 0.8
   </td>
   <td>P0
<p>
Have dependency to Module 2 and 3 (to know which alert is Helpful = true alert)
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>FE/BE
   </td>
   <td>Insight Page
   </td>
   <td>Page to view insight
   </td>
   <td>
<ol>

<li>Add page for users to view Insight

<li>Users can feedback to Alert (Helpful or Not Helpful)

<li>Users can select to manually create or link bug ticket to follow up with the issue > Auto-mark Alert as Helpful
</li>
</ol>
   </td>
   <td>Insight Page 
   </td>
   <td>P0
   </td>
  </tr>
  <tr>
   <td>3
   </td>
   <td>FE/BE
   </td>
   <td>Alert Channel
   </td>
   <td>Have mature alert channel
   </td>
   <td>Users can choose to send alert to email, channel (MM, seatalk)
   </td>
   <td>Support alert channel (MM, seatalk, email)
   </td>
   <td>P1
   </td>
  </tr>
  <tr>
   <td><del>4</del>
   </td>
   <td><del>FE/BE</del>
   </td>
   <td><del>Insight Summary Email </del>
   </td>
   <td><del>Promote Insight system </del>
   </td>
   <td><del>Sent summarised email to Group Owner for tracking point for top 10 pages</del>
   </td>
   <td><del>Automated report </del>
   </td>
   <td><del>P3 - Backlog </del>
   </td>
  </tr>
</table>


<h3>Module 1: ML model to detect anomaly</h3>



<table>
  <tr>
   <td><strong>#</strong>
   </td>
   <td><strong>Requirement</strong>
   </td>
   <td><strong>Details</strong>
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>Apply better ML to accurately detect tracking anomaly, frequency (Daily, Weekly, Monthly and Hourly) 
   </td>
   <td><strong>Objective</strong>: to find outliers (greater mass of data serves primarily to define the "normal" against which anomalies are measured)
<p>
<strong>Model output</strong>
<ol>

<li>Prediction interval: yhat_lower and yhat_upper

<li>Prediction value: y_hat

<li>Trigger alert if <strong>actual value is outside prediction interval</strong>

<li>Training data: E.g. For detection of hourly anomalies, the training period is 2 weeks. For detection of daily anomalies, the training period is 90 days. For detection of weekly anomalies, the training period is 32 weeks.

<li>Limitation: 
<ol>
 
<li>Time-series based anomaly detection uses historical data to flag a single metric within a single dimension value. For example: number of users is low for all region, number of users is low for SG > What about if the anomaly happened for <strong>specific dimension cluster </strong>(e.g. android, app_version 2.89 decrease) but it is cancelled out by other dimension (ios, app_version 2.90 increase) > So the number of users for all regions does not have any anomaly? 
 
<li>For next phase: we may need to implement anomaly detection simultaneously over several metrics and dimension values, at a point in time > How to apply PCA (Principle Component Analysis): <a href="https://visualstudiomagazine.com/articles/2021/10/20/anomaly-detection-pca.aspx">https://visualstudiomagazine.com/articles/2021/10/20/anomaly-detection-pca.aspx</a>

<p>
<strong>Model Evaluation + Parameter Tuning</strong>
<ol>

<li>Check Precision and Recall Score: 
<ol>
 
<li>Precision: Among alerts sent, how many is true alert. PIC: PM & PA
 
<li>Recall: Of all issues, how many are sent by us. PIC: PM & PA
</li> 
</ol>

<li>Sample true alert case

<p>
<strong>Reference</strong>
<p>
 1. FB Prophet: <a href="https://facebook.github.io/prophet/">https://facebook.github.io/prophet/</a>. 
<ol>
 
<li>What it is? Prophet is a procedure for forecasting time series data. Prophet is open source software released by Facebook's Core Data Science team. It is available for download on CRAN and PyPI. It is based on an additive model where non-linear trends are fit with yearly and weekly seasonality, plus holidays. It works best with daily periodicity data with at least one year of historical data. <strong>Prophet is robust to missing data, shifts in the trend, and large outliers</strong> + a simple set of parameters with enormous latitude for tuning
 
<li>Apply FB Prophet to detect anomaly:<a href="https://towardsdatascience.com/forecasting-in-python-with-facebook-prophet-29810eb57e66"> https://towardsdatascience.com/forecasting-in-python-with-facebook-prophet-29810eb57e66</a>
 
<li>Prophet alternative: <a href="https://python.libhunt.com/prophet-alternatives">https://python.libhunt.com/prophet-alternatives</a>
 
<li>Some note: <a href="https://confluence.shopee.io/download/attachments/1361216554/TMS%20Monitor.py?version=1&modificationDate=1665543226000&api=v2">we Monitor.py</a>

<table>
  <tr>
   <td>
<strong>No</strong>
</li> 
</ol>
</li> 
</ol>
</li> 
</ol>
</li> 
</ol>
   </td>
   <td><strong>Note</strong>
   </td>
   <td><strong>Detail</strong>
   </td>
   <td><strong>Demonstration</strong>
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>For data set, start with Prophet()
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td rowspan="3" >2
   </td>
   <td rowspan="3" >For data with clear seasonality, can add:
   </td>
   <td>Additional regressors: They caused the trend to deviate from the baseline, but the trend will stay changed after the event. E.g. Economic Situation
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><strong>Holiday</strong>: They caused the trend to deviate from the baseline, but the trend will NOT changed after the event. E.g. Campaign date 
<ol>
 
<li>Prophet allows for a <strong>lower window and an upper window for holiday</strong> = days to include with the holiday event. E.g. for 9-9 campaign, you can include lower window = -1, upper window = +1 to include +/- 1 day after/before the campaign
</li> 
</ol>
   </td>
   <td>



<pre class="prettyprint">campaign = pd.DataFrame({
  'holiday': 'campaign',
  'ds': pd.to_datetime(['2022-01-01', '2022-02-02', '2022-03-03',
                        '2022-04-04', '2022-05-05', '2022-06-06',
                        '2022-07-07', '2022-08-08', '2022-08-08',
                        '2022-09-09', '2022-10-10', '2022-11-11',
                        '2022-12-12']),
  'lower_window': -1,
  'upper_window': 1,
})
app_release = pd.DataFrame({
  'holiday': 'app_release',
  'ds': pd.to_datetime(['2022-10-07']),
  'lower_window': -1,
  'upper_window': 0,
})
holidays = pd.concat((campaign, app_release))
prophet = Prophet(holidays=holidays)</pre>


   </td>
  </tr>
  <tr>
   <td><strong><a href="https://github.com/dr-prodigy/python-holidays">Country holiday </a>: </strong>
<p>
A list of available countries, and the country name to use, is available on their page: <a href="https://github.com/dr-prodigy/python-holidays">https://github.com/dr-prodigy/python-holidays</a>. In addition to those countries, Prophet includes holidays for these countries: Brazil (BR), Indonesia (ID), India (IN), Malaysia (MY), Vietnam (VN), Thailand (TH), Philippines (PH), Pakistan (PK), Bangladesh (BD), Egypt (EG), China (CN), and Russian (RU), Korea (KR), Belarus (BY), and United Arab Emirates (AE).
   </td>
   <td>



<pre class="prettyprint">prophet = Prophet(holidays=holidays)
prophet.add_country_holidays(country_name='SG')</pre>


   </td>
  </tr>
  <tr>
   <td>3
   </td>
   <td>For data with change point (real time series frequently have abrupt changes), can:
   </td>
   <td>Adjust <a href="https://facebook.github.io/prophet/docs/trend_changepoints.html">changepoint_prior_scale</a>. Decreasing it will make the trend <em>less</em> flexible (meaning that the prediction interval is smaller)
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>4
   </td>
   <td>Set Importance score
   </td>
   <td>
<ol>

<li>Importance score = Difference between lower prediction value (yhat_lower)/upper prediction value (yhat_upper) and actual value (fact)

<li>To achieve the target (80% of alert is marked as helpful + Detect 50% of issues): we can set to send alert when importance score > 0.2 for example.
</li>
</ol>
   </td>
   <td>



<pre class="prettyprint">forecasted.loc[forecasted['anomaly'] ==1, 'importance'] = \
        (forecasted['fact'] - forecasted['yhat_upper'])/forecast['fact']
    forecasted.loc[forecasted['anomaly'] ==-1, 'importance'] = \
        (forecasted['yhat_lower'] - forecasted['fact'])/forecast['fact']</pre>


   </td>
  </tr>
</table>


2. Meituan: [https://tech.meituan.com/2017/04/21/order-holtwinter.html](https://tech.meituan.com/2017/04/21/order-holtwinter.html)

3. Google Analytics: [https://support.google.com/analytics/answer/9517187?hl=en&ref_topic=10333392](https://support.google.com/analytics/answer/9517187?hl=en&ref_topic=10333392) 

   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
</table>


<h3>Module 2: Insight page</h3>



<table>
  <tr>
   <td><strong>#</strong>
   </td>
   <td><strong>Requirement</strong>
   </td>
   <td><strong>Details</strong>
   </td>
   <td><strong>UI Screenshots</strong>
   </td>
  </tr>
  <tr>
   <td>2a
   </td>
   <td>Entry point
   </td>
   <td><strong>Entry Point</strong>
<ol>

<li>Report Overview page: Show latest insight (random selection)

<li>Insight page 

<p>
<strong>Access Control</strong>
<ol>

<li>Only show alert for regions that users have access to (Apply access via SOUP)

<p>
<strong>Traffic Insight Tooltip</strong>
<ol>

<li>Read more: <a href="https://sites.google.com/shopee.com/traffic-suite-help-center/tms/tms-monitor">How we identify anomaly </a>
</li>
</ol>
</li>
</ol>
</li>
</ol>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>2b
   </td>
   <td>Insight page
   </td>
   <td>2 tabs: My Insight and All Insight
<ol>

<li><strong>My insight</strong>: display all insights created by users or sent to users (via email or seatalk or mattermost)

<li><strong>All insight</strong>: all insights by other users,<del> except My Insight.</del> 
<ol>
 
<li>Users can click "Join" to subscribe to others insight  
<ol>
  
<li>Move the insight to "My Insight" tab. Status changed from "Join" To "Joined"
  
<li>Add users email to Alert 
</li>  
</ol>
 
<li>Alert Owner can not click to Un-join. Need to disable the alert from Traffic Alert page. Tooltip: "This is alert created by you. Please go to <a href="http://traffic%20alert/">Traffic Alert</a> to disable "

<p>
Note:
<ol>

<li>There are <strong>7 default insights</strong> (Daily Active Users, Daily Active Devices, New Users...) -> will trigger by default, even though no users create alert for these.

<li>Other alerts will only trigger if >=1 user created alert.

<li>Possible Explanation: Compare to Click/Impression for same page_type, page_section, point, target_type, target_type_ext <strong>(if any)</strong>. For example, if users create Alert for <strong>Impression of Product, YMAL, item</strong>; possible explanation will compare to <strong>Click of Product, YMAL, item</strong>

<li>Possible Explanation: Compare to view of same page_type <strong>(if any)</strong>. For example, if users create Alert for <strong>Impression of Product, YMAL, item</strong>; possible explanation will compare to <strong>View of Product.</strong>

<table>
  <tr>
   <td>
<strong>No.</strong>
</li>
</ol>
</li>
</ol>
</li>
</ol>
   </td>
   <td><strong>Metrics</strong>
   </td>
   <td><strong>Possible Explanation (General Breakdown) - Only shown if there is anomaly</strong>
   </td>
   <td><strong>Possible Explanation (Specific) - Show even if no anomaly </strong>
   </td>
   <td><strong>Note</strong>
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>Record Count
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to Record Count for other operation (Click, View, Impression) of the same page_type, page_section, point, target_type, target_type_ext
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>User Count
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to User Count for other operation (Click, View, Impression) of the same page_type, page_section, point, target_type, target_type_ext
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>3
   </td>
   <td>Count per User 
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to Count per User for other operation (Click, View, Impression) of the same page_type, page_section, point, target_type, target_type_ext
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>4
   </td>
   <td>CTR - Record Count
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to numerator and dominator
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>5
   </td>
   <td>CTR User Count
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to numerator and dominator
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>6
   </td>
   <td>Active User
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version

<li>Page View User Count
</li>
</ol>
   </td>
   <td>
   </td>
   <td>we Default,
<p>
Freq: Daily
   </td>
  </tr>
  <tr>
   <td>7
   </td>
   <td>Active Device
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version

<li>Page View Device Count
</li>
</ol>
   </td>
   <td>
   </td>
   <td>we Default,
<p>
Freq: Daily
   </td>
  </tr>
  <tr>
   <td>8
   </td>
   <td>User Sign Up Success: <a href="https://tms.traffic.shopee.io/tracker/point?appId=0&pointId=7832">https://we.traffic.shopee.io/tracker/point?appId=0&pointId=7832</a>
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to:
<ol>

<li>Active User

<li>Active Device
</li>
</ol>
   </td>
   <td>we Default,
<p>
Freq: Daily
   </td>
  </tr>
  <tr>
   <td>9
   </td>
   <td>User Login Success:
<p>
<a href="https://tms.traffic.shopee.io/tracker/point?appId=0&pointId=11756">https://we.traffic.shopee.io/tracker/point?appId=0&pointId=11756</a>
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to:
<ol>

<li>Active User

<li>Active Device
</li>
</ol>
   </td>
   <td>we Default,
<p>
Freq: Daily
   </td>
  </tr>
  <tr>
   <td>10
   </td>
   <td>Users Product Page View: <a href="https://tms.traffic.shopee.io/tracker/point?appId=0&pointId=4911">https://we.traffic.shopee.io/tracker/point?appId=0&pointId=4911</a>
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to:
<ol>

<li>Active User

<li>Active Device

<li>User Sign Up Success

<li>User Login Success
</li>
</ol>
   </td>
   <td>we Default,
<p>
Freq: Daily
   </td>
  </tr>
  <tr>
   <td>11
   </td>
   <td>Users Add to cart success: <a href="https://tms.traffic.shopee.io/tracker/point?appId=0&pointId=1464">https://we.traffic.shopee.io/tracker/point?appId=0&pointId=1464</a>
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to:
<ol>

<li>Active User

<li>Active Device

<li>Product Page View
</li>
</ol>
   </td>
   <td>we Default,
<p>
Freq: Daily
   </td>
  </tr>
  <tr>
   <td>12
   </td>
   <td>Users Checkout success: <a href="https://tms.traffic.shopee.io/tracker/point?appId=0&pointId=7769">https://we.traffic.shopee.io/tracker/point?appId=0&pointId=7769</a>
   </td>
   <td>
<ol>

<li>Platform

<li>Region

<li>App version

<li>RN version
</li>
</ol>
   </td>
   <td>Compare to:
<ol>

<li>Active User

<li>Active Device

<li>Product Page View

<li>Add to Cart Success
</li>
</ol>
   </td>
   <td>we Default,
<p>
Freq: Daily
   </td>
  </tr>
</table>


**Example**


<table>
  <tr>
   <td><strong>Alert</strong>:
<p>
Record Count for (Impression, Product, YMAL, Item) is high on 15 Sep 2022.
<p>
It has the value of 10M, which is outside the expected range of 1M-5M
   </td>
  </tr>
  <tr>
   <td><strong>Possible explanation</strong>
<ol>

<li>Impression Record Count for (Product, YMAL, Item) is unexpectedly high for <strong>Android</strong>. It has the value of 5M, which is outside the expected range of 1M-2M

<li>Impression Record Count for (Product, YMAL, Item) is unexpectedly high for <strong>ID, SG</strong>. ID has the value of 5M, which is outside the expected range of 1M-2M. SG has the value of 3M, which is outside the expected range of 1-1.5M

<li>Compared to Record Count of (Product, YMAL, item), Click for (Product, YMAL, item) has no abnormal change.

<li>Compared to Record Count of (View, Product), View for Product page has no abnormal change.
</li>
</ol>
   </td>
  </tr>
</table>


**Access Control**



1. Only show alerts for regions that users have access to (Apply access via SOUP).
2. For example:
    1. user region = SG > "Active User" alert for SG only
    2. user region = ID, MY > "Active User" alert for ID, MY only
    3. user region = Regional > "Active User" alert for All regions

**Insight History: **will show insight that meet 1 of the following condition:



1. Insight triggered last 7 days
2. Insight did not have feedback yet (Helpful/Not Helpful/Report Issue) and triggered within last 14 days

**UI Note**:



1. Alert message and Possible Explanation format.
    1. ~~If % change is >= 200%, display times instead. For example: % change = 200 → display "2 times more than expected". % change = -400% → display "4 times less than expected"~~
    2. If the number of values under breakdown is too many (e.g. for app_version) → set limitation to only display top 10 abnormal value (top 10 latest apps) and number of records/users is at least >=1000
    3. If the alert message for 1 region only (in case users only have access for 1 region) > For possible explanation, no need to show possible explanation for platform, as it is duplicated.

<table>
  <tr>
   <td>
<strong>Line chart legend</strong>
<p>
"Actual" vs "Expected Range" vs % diff
<p>
<strong>Actual</strong>: actual value. E.g. 10M
<p>
<strong>Expected range</strong>: yhat_lower - yhat_upper. E.g. 1M-5M
<p>
<strong>%</strong>: = (actual - y_hat)/y_hat. E.g = (10-3)/3 = +233%
<p>
<strong>Size of the abnormal dot</strong> (red dot): bigger as important score increases. Important score = difference between actual value and expected range.
<ul>

<li>If actual > yhat_upper: Importance = (actual - yhat_upper)/yhat_upper 

<li>If actual < yhat_lower: Importance = (yhat_lower - actual)/yhat_upper 
</li>
</ul>
   </td>
  </tr>
  <tr>
   <td><strong>Alert Message/Title</strong>
<p>
[Region]{Metrics} is {high/low} on {date:hour}
<p>
It has the value of {value}, which is outside the expected range of {expected range} and {% change}  {less than/more than} expected
   </td>
  </tr>
  <tr>
   <td><strong>Possible explanation</strong>
<p>
<strong>1. If there is anomaly:</strong>
<p>

    {Metrics} for {Breakdown dimension} is {high/low} on {date:hour}
<p>

    {Breakdown dimension} has the value of {value}, which is outside the expected range of {expected range} and {% change}  {less than/more than} expected 
<p>
<strong>2. If there is no anomaly:</strong>
<p>

    {Metrics} for {Breakdown dimension} has no anomaly on {date:hour}
<p>

    Compare to {tracking point}, {Breakdown dimension} has the value of {value}, which is inside the expected range of {expected range}
   </td>
  </tr>
</table>


2. Example

<span style="text-decoration:underline;">Alert Message/Title:</span>

Record Count for (Impression, Product, YMAL, Item) is high on 15 Sep 2022.

It has the value of 10M, which is outside the expected range of 1M-5M

<span style="text-decoration:underline;">Possible explanation</span>



    1. Record Count for (Impression, Product, YMAL, Item) is unexpectedly high for **Android**. It has the value of 5M, which is outside the expected range of 1M-2M
    2. Record Count for (Impression, Product, YMAL, Item) is unexpectedly high for **ID, SG**. ID has the value of 5M, which is outside the expected range of 1M-2M. SG has the value of 3M, which is outside the expected range of 1-1.5M
    3. Compared to Record Count of (Impression, Product, YMAL, item), Click for (Product, YMAL, item) has no abnormal change.
    4. Compared to Record Count of (View, Product), View for Product page has no abnormal change.

3. Highlight app-release date/holiday/campaign date

4. Have link to we tracking library

   </td>
   <td>

Note: There are 2 UI options for the Insight detail page



1. Insight detail page is a new page
    1. Pros: Insight detail page link can be easily shared.
    2. Cons: Maybe too big space for 1 insight card. Opening a new page or enter insight detail page is maybe disrupting users flow (users are no longer in Insight Summary page)
2. Insight detail is a drawer: [https://3x.ant.design/components/drawer/#header](https://3x.ant.design/components/drawer/#header)
    3. Pros: Enough space for 1 insight card. Opening drawer still keep users to be in Insight Summary page.
    4. Cons: Need to check if we can **share** Insight detail drawer: When users enter Insight detail drawer link, can highlight Insight card + Open drawer

Option 2: 

   </td>
  </tr>
  <tr>
   <td>2c

   </td>
   <td>Feedback (Helpful/Not Helpful)

   </td>
   <td>


1. If users choose Not Helpful, ask users to select reason why: (Users can only select 1 option = Single selection) 
    1. The Insight is hard to find in we.
    2. The insight is not correct.
    3. I see the same insight again and again in we.
    4. I already knew this insight.
    5. Something else:
        1. Can type to add detail
        2. Maximum character: max 300 (Show 0/300)
2. If users select Not Helpful > Helpful is marked right away > Open Feedback Popup
3. Feedback Popup:
    6. Default = No Option is selected, only Dismiss button shown
    7. If users select 1 option (except Something else), Submit button shown
    8. If users select Something else, text editor shown
    9. If users add text under Text Editor, Submit button shown 
4. If users click Helpful/Not Helpful icon, users **can NOT double click **to remove the selection.
    10. After users click Helpful/Not Helpful icon, he/she must choose 1 option (Helpful/Not Helpful)
    11. User can change selection. E.g. from Helpful to Not Helpful, from Not Helpful to Helpful. 
    12. User can change Not Helpful reason. For example:
        3. 1st time: users select Not Helpful - reason = "This Insight is not correct" > Submit
        4. Next time users can click Not Helpful again, change reason from "This Insight is not correct" to Something else. (Need to show latest reason users select)
5. Show the number of Helpful alert among Total (to check the alert performance and promote features) 
    13. % of Helpful alert = Number of alert that >= 1 user(s) marked as Helpful/Total Alert
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>
2d

   </td>
   <td>Report Issue

   </td>
   <td>Users can select to **manually create** or **link bug ticket** to follow up with the issue > we will auto-mark Alert as Helpful

**Create JIRA ticket**: Similar UI as current [Report Issue](https://sites.google.com/shopee.com/traffic-suite-help-center/tms/user-behaviour-track-tms-design/tracking-library/report-tracking-point-issue?authuser=0) 


<table>
  <tr>
   <td><strong>Fields</strong>
   </td>
   <td><strong>Value</strong>
   </td>
  </tr>
  <tr>
   <td>Project
   </td>
   <td><a href="https://jira.shopee.io/projects/SPTSS/">https://jira.shopee.io/projects/SPTSS/</a>
   </td>
  </tr>
  <tr>
   <td>Issue Type
   </td>
   <td>Problem
   </td>
  </tr>
  <tr>
   <td>Summary
   </td>
   <td>[we Traffic Insight] Alert message.
<p>
E.g. [we Traffic Insight] Number of users is low on 16 Oct 2022.
   </td>
  </tr>
  <tr>
   <td>Priority
   </td>
   <td>
<ol>

<li>Possible value = Highest/High/Medium/Low

<li>Default = Low

<li>Remove the tooltip 
</li>
</ol>
   </td>
  </tr>
  <tr>
   <td>Attachment
   </td>
   <td>Insight Screenshot
   </td>
  </tr>
  <tr>
   <td>Description(1)
   </td>
   <td>User can input more
<p>
Affected region: we filled
<p>
Affected platform: we filled
<p>
Affected app version: we filled 
<p>
Affected RN version: we filled 
   </td>
  </tr>
  <tr>
   <td>Description(2)
   </td>
   <td>-Separated by dash line from Description(1)
<p>
Tracking point list (we auto-filled)
   </td>
  </tr>
</table>


**Link JIRA ticket**: Similar UI as current [Report Issue](https://sites.google.com/shopee.com/traffic-suite-help-center/tms/user-behaviour-track-tms-design/tracking-library/report-tracking-point-issue?authuser=0) 



1. Project: single select + auto-suggestion
2. Link tracking point: we auto-filled
3. Attachment: Insight Screenshot

**UI note: **If the insight is already reported by user A, the ticket will be shown for same Insight that are displayed to user B,C,D... > so no duplicated ticket can be created.

   </td>
   <td>Link JIRA: 

Create JIRA: 

   </td>
  </tr>
</table>


<h3>Module 3: Alert Channel</h3>



<table>
  <tr>
   <td><strong>#</strong>
   </td>
   <td><strong>Requirement</strong>
   </td>
   <td><strong>Details</strong>
   </td>
   <td><strong>UI Screenshots</strong>
   </td>
  </tr>
  <tr>
   <td>3a
   </td>
   <td>Alert Channel 
   </td>
   <td>Alert won't be sent to PIC automatically (except for we default alert)
<p>
Only if users add alert, alert will be sent to PIC via selected channel:
<ol>

<li>Email

<li>Sea talk

<li>Mattermost

<li>Whatever alert channel (email/seatalk/MM) users selected, alert will be displayed in we Alert Insight page.

<li>Users have to create alert for we Default metrics (Daily Active users) to send alert to their email/seatalk/mattermost. If not, we default only display in we insight page, but not send to emails/seatalk/mattermost.

<p>
<strong>Default tab</strong>: My Alert. Note: Tab shows the number of alert
</li>
</ol>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>3b
   </td>
   <td>Create alert
   </td>
   <td><strong>Evaluation frequency</strong>
<ol>

<li>Daily (default)

<li>Weekly 

<li>Monthly

<li>Hourly

<p>
<strong>Metrics</strong>
<ol>

<li>Record Count 

<li>User Count 

<li>Count per user 

<li>CTR Count (choose numerator/denominator) 

<li>CTR User Count (choose numerator/denominator) 

<li>AU (Active User) 

<li>AD (Active Device)

<li>User Signup success

<li>User Login success

<li>Users Product Page View

<li>Users Add to cart success

<li>Users Checkout Success

<p>
<strong>Filter (Optional): </strong>to help users to focus on a particular segment. All should have <strong>auto-suggestion</strong>.
<ol>

<li>Platform: = Android/IOS.  
<ol>
 
<li>Support condition: = (multiple selection)
</li> 
</ol>

<li>Region = ID... 
<ol>
 
<li>Support condition: = (multiple selection)
</li> 
</ol>

<li>App version = 2.92, 2.93... 
<ol>
 
<li>Support condition: = (multiple selection)
 
<li>Support condition: >= OR <= (single selection)
</li> 
</ol>

<li>RN Version = xxx 
<ol>
 
<li>Support condition: = (multiple selection)
 
<li>Support condition: >= OR <= (single selection)

<p>
<strong>Tracker_ID (Compulsory): </strong>users have to choose which tracking point to monitor
<ol>

<li>Select Tracker ID to monitor. 
<ol>
 
<li>Support search bar
 
<li>Support multiple selection
</li> 
</ol>

<li>Set restriction: For we default alert (Active user, User Signup success...), don't need to select Tracker ID → remove trackerID selection.

<p>
<strong>Condition: </strong>Support only we model to focus first.
<ol>

<li>Has anomaly (we model): Default option, as users rely more on auto-detection model than self-defined rules.

<li><del>Is less than or equal to (<=)</del>

<li><del>Is greater than or equal to (>=)</del>

<li><del>% increase more than </del>

<li><del>% decrease more than</del>

<li><del>% change is more than</del>

<p>
<strong>Alert name</strong>
<ol>

<li>Auto generated = [Metrics] [Condition] for [Tracker]

<li>For example: If users select Metrics = Record Count; Condition = Has anomaly; Tracker = View, Product | View, me page → Alert name = Record Count has anomaly for View, Product | View, me page

<li>Users can click to edit the Alert name 

<p>
<strong>Access Control</strong>
<ol>

<li>Users can only select regions that users have access to (apply access via SOUP) > Auto-suggestion only regions that users have access to 

<li><span style="text-decoration:underline;">Can put in backlog</span>: Users can create alerts for tracking point of the group that they joined ONLY (show tracker_id from users' group ONLY) → integrate Group Autonomy 
<ol>
 
<li>Dependencies: Group Autonomy flow to claim old tracking points (end Nov) need to be done before we Monitor Insight.
 
<li>If integrate with Group Autonomy, TrackerID search should have tooltip to let user know to join group before creating alert. 
</li> 
</ol>
</li> 
</ol>
</li> 
</ol>
</li> 
</ol>
</li> 
</ol>
</li> 
</ol>
</li> 
</ol>
</li> 
</ol>
</li> 
</ol>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>3c
   </td>
   <td>Email/Seatalk/Mattermost template
   </td>
   <td>
<ol>

<li><del>Insight will keep sending to users Email/Seatalk/Mattermost until 1 of those condition is met: </del> 
<ol>
 
<li><del>Users marked Insight as Helpful/Not Helpful</del>
 
<li><del>Users report issue </del>
 
<li><del>14 days passed since Alert trigger date </del>
</li> 
</ol>

<li>Insights sent to Email/Seatalk/Mattermost only contain key alert messages. Users can click image to view Insight detail (Possible Explanation) in we

<li>Link in email 
<ol>
 
<li> we News Feed: <a href="https://trafficsuite.shopee.io/news">https://trafficsuite.shopee.io/news</a>
 
<li>we Support Group: <a href="https://sites.google.com/shopee.com/traffic-suite-help-center/home#h.s2rit6csc6n2">https://sites.google.com/shopee.com/traffic-suite-help-center/home#h.s2rit6csc6n2</a>
 
<li>we Help Centre: <a href="https://sites.google.com/shopee.com/traffic-suite-help-center/tms?authuser=0">https://sites.google.com/shopee.com/traffic-suite-help-center/we</a> 
</li> 
</ol>

<li><del>Set limitation </del> 
<ol>
 
<li><del>Number of email sent per users should not > 10 per day</del>
 
<li><del>Number of seatalk/mattermost message sent to users should not > 10 per day</del>
</li> 
</ol>
</li> 
</ol>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>3d
   </td>
   <td>Handle different alert for same metrics
   </td>
   <td>E.g. 
<ol>

<li>User A created Alert (1): User Count for "Product, YMAL, item", region = ID, condition: % change is more than 5%

<li>User B created Alert (2): User count for "Product, YMAL, item", region = SG, condition: has anomaly (we model)

<li>Question: 
<ol>
 
<li>How should we display the Insight for user A and B? > <strong>display alert to user A and B if they meet their trigger condition.</strong>
 
<li>Can user A create Alert for same metrics + Filter + Tracker ID, but with different condition? > Yes
 
<li>Alert (1) and Alert (2) are counted as 1 Insight card? > No, Insight card is unique by Metrics + Filter condition + Tracker ID + Condition
</li> 
</ol>
</li> 
</ol>
   </td>
   <td>N/A
   </td>
  </tr>
</table>


<h3>Module 5: Other we Monitor changes</h3>



<table>
  <tr>
   <td><strong>#</strong>
   </td>
   <td><strong>Requirement</strong>
   </td>
   <td><strong>Details</strong>
   </td>
   <td><strong>UI Screenshots</strong>
   </td>
  </tr>
  <tr>
   <td>5a
   </td>
   <td>To <strong>save resources</strong>, remove Volume Alert feature in we
   </td>
   <td>
<ol>

<li>Remove<strong> [we] Daily Alert Report </strong>email<strong>   \
</strong> 
<ol>
 
<li>Add we users <strong>(who add tracking to favourite)</strong> as JIRA ticket's Request participants/People Involved > users will receive email notification (sent by JIRA, not we)
 
<li>For validation alert, have new email template (Module 5b)
 
<li>For system notification → remove emails
 
<li>For volume Alert → replaced by we Insight email (Module 3c)
</li> 
</ol>

<li>Remove Volume Alert from <strong>Alert tab</strong> in Tracking Library

<li>Remove Volume Alert in TrackingPoint detail page

<li>Remove Volume Alert in Notification

<li>Move Alert Report in we: <a href="https://tms.traffic.shopee.io/monitor/trackingAlert/validationAlert">https://we.traffic.shopee.io/monitor/trackingAlert/validationAlert</a> to Admin menu > Change to Validation Alert Report (Remove Volume Alert Tab)

<li><del> Move Tracking Issue Summary's content in we <a href="https://tms.traffic.shopee.io/monitor/trackingIssueSummary">https://we.traffic.shopee.io/monitor/trackingIssueSummary</a> (PIC: Benley) to be Under Admin</del> 
<ol>
 
<li><del>Tooltip: Coming Soon. Please check <a href="https://jira.shopee.io/projects/SPTSS/">JIRA board </a>for tracking issue list.</del>
 
<li><del>Yong Chao team(?) will take over Tracking Issue Monitoring Report > will design Tracking Issue Summary page later </del>
</li> 
</ol>
</li> 
</ol>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>5b
   </td>
   <td>New email template for Validation Alert 
   </td>
   <td>Template ID: EMXXXX
<p>
<strong>Recipient</strong>: Users who add tracking point to Favorite, Group Owner (including Tracking Point PM)
<p>
<strong>Subject</strong>: [we] Validation Alert for [tracking point] on [date]!
<p>
<strong>Content</strong>:

<table>
  <tr>
   <td><strong>Email Section</strong>
   </td>
   <td><strong>Content</strong>
   </td>
  </tr>
  <tr>
   <td>Summary
   </td>
   <td>Validation Alert for [tracking point] on [date]!
   </td>
  </tr>
  <tr>
   <td>Tracker ID
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Tracker Name
   </td>
   <td>app_id:
<p>
operation:
<p>
page_type:
<p>
page_section:
<p>
target_type:
   </td>
  </tr>
  <tr>
   <td>Owner Group
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>PM
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Description
   </td>
   <td>Error message:
<p>
Sample Error Rate:
<p>
Alert Platform:
<p>
Alert Region:
<p>
App Version:
   </td>
  </tr>
  <tr>
   <td>Action Button
   </td>
   <td>View Detail → Direct to tracking point detail page | Alert tab
<p>
Learn More → Hyper link: <a href="https://sites.google.com/shopee.com/traffic-suite-help-center/tms/tms-monitor/validation-alert?authuser=0">https://sites.google.com/shopee.com/traffic-suite-help-center/we/we-monitor/validation-alert?authuser=0</a>
   </td>
  </tr>
</table>


Note:

+ 1 validation alert email for each tracking point 

+ Email will be sent until users fixed the validation issue (e.g. in new app version) → maybe spam for users

   </td>
   <td>
   </td>
  </tr>
</table>


<h2>FAQ</h2>



<table>
  <tr>
   <td><strong>Questions</strong>
   </td>
   <td><strong>From</strong>
   </td>
   <td><strong>Answers</strong>
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>hang.le
   </td>
   <td>Do we need to store historical alert? > How about displaying the latest 7 days alert as the first stage? 

<table>
  <tr>
   <td><strong>Option</strong>
   </td>
   <td><strong>Pro</strong>
   </td>
   <td><strong>Cons</strong>
   </td>
  </tr>
  <tr>
   <td>If only display latest day alert
   </td>
   <td>
<ol>

<li>Users have incentive to go to we to check every day and monitor alert closely

<li>Simple and clear 

<li>If it is metrics that are important to users, it gives incentive to  users to create alerts for it to receive email instead.

<li>When actual issues happened, it is <strong>NOT</strong> recommended that users check historical insight to ask why there are no insights at this date (that real issue happened) > It is consistent to our direction to detect true alerts but did not guarantee to detect all alerts.
</li>
</ol>
   </td>
   <td>
<ol>

<li>Maybe hard for the internal team to check model performance overtime. E.g to detect why some really obvious drop is not shown. 

<li>What if the alert happened at 11PM 20 Sep > Then if users check data for 21 Sep, we stop display the data >> Users don't know about the alert and only have 1 hour to check the insight
</li>
</ol>
   </td>
  </tr>
  <tr>
   <td>If display historical alert
   </td>
   <td>
<ol>

<li>Check model performance overtime. E.g to detect why some really obvious drop is not shown. 
</li>
</ol>
   </td>
   <td>Alert for different date may look confusing > need to have ways to filter alert for different dates in UI
   </td>
  </tr>
</table>


   </td>
  </tr>
  <tr>
   <td>2

   </td>
   <td>liuyuan.su

   </td>
   <td>How can we ensure all alerts have feedback, so we can keep improving model performance?

Suggestion:



1. we will keep sending email/seatalk for alert UNTIL users gave alert feedback (e.g. Helpful, Report issue) or pass 14 days
   </td>
  </tr>
  <tr>
   <td>
3

   </td>
   <td>hang.le

   </td>
   <td>What happened if we detect anomaly in continuous date, e.g. 1 Oct, 2 Oct, 3 Oct, 4 Oct

Instead of displaying 4 alerts for all dates on 4 Oct, Can we group the alert to be as below?

"Number of users is low on 1 Oct 2022" = 1 alert shown on 1 Oct.

"Number of users is low from 1 Oct and 2 Oct" = 1 alert shown on 2 Oct.

"Number of users is low from 1 Oct and 3 Oct" = 1 alert shown on 3 Oct.

"Number of users is low from 1 Oct and 4 Oct" = 1 alert shown on 4 Oct.

   </td>
  </tr>
</table>


<h2>Backlog feature </h2>


<h3>Module 4: Weekly Alert Report</h3>



<table>
  <tr>
   <td><strong>#</strong>
   </td>
   <td><strong>Requirement</strong>
   </td>
   <td><strong>Details</strong>
   </td>
   <td><strong>Note</strong>
   </td>
  </tr>
  <tr>
   <td>4a
   </td>
   <td>Weekly report sent to tracking point group owner (Pilot)
   </td>
   <td><strong>Recipient: </strong>Group Owner for tracking point of top 10 pages
   </td>
   <td>This feature is put in backlog due to:
<ol>

<li>Use Case is not clear/must-have: send weekly report to bring visibility? ask PIC to follow up?).

<li>Insight model performance is not clear.
</li>
</ol>
   </td>
  </tr>
</table>


<h3>Module 6: Link Insight Alert for tracking point detail page</h3>



<table>
  <tr>
   <td><strong>#</strong>
   </td>
   <td><strong>Requirement</strong>
   </td>
   <td><strong>Details</strong>
   </td>
   <td><strong>Note</strong>
   </td>
  </tr>
  <tr>
   <td>6a
   </td>
   <td>Add Insight tab for tracking point detail page
   </td>
   <td>
<ol>

<li>Insight tab under for Tracking point detail page

<li>Add Insight in Alert tab in Tracking Library page
</li>
</ol>
   </td>
   <td>This feature is put in backlog due to:
<ol>

<li>Use case is not clear/must-have: Add Insight tab to bring visibility more to the Insight? 

<li>Not match function's definition. The Insight System is a system for users to create personalised alert > Personal page. It is not compulsory that each tracking point will have Alert. But if we add Insight Tab in Tracking point detail page, it is understood that Insight will be triggered for all tracking points.
</li>
</ol>
   </td>
  </tr>
</table>


<h2>Appendix</h2>


<h3>Key Tracking Metrics </h3>



<table>
  <tr>
   <td><strong>Report link</strong>
   </td>
   <td><strong>Subject</strong>
   </td>
   <td><strong>Metrics</strong>
   </td>
   <td><strong>Metrics Type</strong>
   </td>
   <td><strong>Calculation Logics</strong>
   </td>
   <td><strong>Breakdown by (Common)</strong>
   </td>
   <td><strong>Breakdown by (Specific)</strong>
   </td>
   <td><strong>Frequency</strong>
   </td>
   <td><strong>Add in Milestone 1?</strong>
   </td>
  </tr>
  <tr>
   <td rowspan="4" >Notification North Star Dashboard (1st tab)
<p>
<a href="https://datasuite.shopee.io/dashboard/dashboard/25b334ec-086a-479c-a18a-154e27ca27f5/normal?page=0">https://datasuite.shopee.io/dashboard/dashboard/25b334ec-086a-479c-a18a-154e27ca27f5/normal?page=0</a>
<p>
Note:
<ol>

<li>Dashboard shows data T-3

<li>SQL <a href="https://confluence.shopee.io/download/attachments/1361216554/new_ar_north_star_script_optimized.py?version=1&modificationDate=1662531419000&api=v2">new_ar_north_star_script_optimized.py</a>
</li>
</ol>
   </td>
   <td>AR
   </td>
   <td><strong>AR Click per Noti Session</strong> 
   </td>
   <td>Float
<p>
E.g. 0.58
   </td>
   <td>When user visits Shopee and comes to the notification page, how many ARs do they click on average
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>AR
   </td>
   <td><strong>% of Noti Session</strong> 
   </td>
   <td>%
<p>
E.g. 14.9%
   </td>
   <td>When a user visits Shopee, what is the probability that they will come to the notification page.
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily 
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>AR
   </td>
   <td><strong>Unread Noti Folder CTR</strong> 
   </td>
   <td>%
<p>
E.g. 0.4%
   </td>
   <td>Session level CTR for each folder when there is unread dot
   </td>
   <td>Region
   </td>
   <td>Noti Folder (data field)
<p>
case when page_section[0] = 'order_updates' and target_type = 'read_all_button' then 'order_updates'  \
                 else get_json_object(data,'$.noti_folder') end as <strong>noti_folder</strong>,
   </td>
   <td>Daily 
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>AR
   </td>
   <td><strong>AR Sent per User</strong>
   </td>
   <td>Float
<p>
E.g. 3.7
   </td>
   <td>AR sent per user
   </td>
   <td>Region
   </td>
   <td>Folder (data field)
   </td>
   <td>Daily
   </td>
   <td>N
<p>
BE data
   </td>
  </tr>
  <tr>
   <td rowspan="6" >Notification North Star Dashboard (2nd tab)
<p>
<a href="https://datasuite.shopee.io/dashboard/dashboard/25b334ec-086a-479c-a18a-154e27ca27f5/normal?page=0">https://datasuite.shopee.io/dashboard/dashboard/25b334ec-086a-479c-a18a-154e27ca27f5/normal?page=0</a>
<p>
Note:
<ol>

<li>Dashboard shows data T-3

<li>SQL <a href="https://confluence.shopee.io/download/attachments/1361216554/new_pn_north_star_script_optimized.py?version=1&modificationDate=1662533102000&api=v2">new_pn_north_star_script_optimized.py</a>
</li>
</ol>
   </td>
   <td>PN
   </td>
   <td><strong>% A1 Sent PN</strong>
   </td>
   <td>% 
<p>
E.g. 93%
   </td>
   <td>% of active buyers that Shopee sent PN to today
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>N
<p>
BE data
   </td>
  </tr>
  <tr>
   <td>PN
   </td>
   <td><strong>% A1 who clicked PN</strong>
   </td>
   <td>% 
<p>
E.g. 9.9%
   </td>
   <td>% of active buyers who clicked PN at least once today
   </td>
   <td>Region
   </td>
   <td>Folder (data field)
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>PN
   </td>
   <td><strong>% A1 1st Enter From PN</strong>
   </td>
   <td>%
<p>
E.g. 4.8%
   </td>
   <td>% of active buyers who entered Shopee from PN
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>PN
   </td>
   <td><strong>PN CTR/UCTR</strong>
   </td>
   <td>%
<p>
E.g. 0.8%
   </td>
   <td>PN CTR = # of PN clicked / # of PN sent
<p>
PN UCTR = # of unique users who clicked PN / # of unique users who we sent PN to
   </td>
   <td>Region
   </td>
   <td>Folder (data field)
   </td>
   <td>Daily 
   </td>
   <td>N
<p>
BE data
   </td>
  </tr>
  <tr>
   <td>PN
   </td>
   <td><strong>In-App Block PN</strong> 
   </td>
   <td>%
<p>
E.g. 3.5%
   </td>
   <td>user toggle off PN from App setting
   </td>
   <td>Region
   </td>
   <td>Folder
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>PN
   </td>
   <td><strong>Device Block PN </strong>
   </td>
   <td>%
<p>
E.g. 10.3%
   </td>
   <td>user block Shopee PN from their device
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td rowspan="5" >Notification North Star Dashboard (4th tab)
<p>
<a href="https://datasuite.shopee.io/dashboard/dashboard/25b334ec-086a-479c-a18a-154e27ca27f5/normal?page=0">https://datasuite.shopee.io/dashboard/dashboard/25b334ec-086a-479c-a18a-154e27ca27f5/normal?page=0</a>
<p>
Note:
<ol>

<li>Dashboard shows data T-3

<li>SQL 
</li>
</ol>
   </td>
   <td>Sharing Panel
   </td>
   <td><strong>Page Visit</strong> 
   </td>
   <td>Int
<p>
E.g. 3M
   </td>
   <td>number of session cnt when users enter sharing panel
   </td>
   <td>Region
   </td>
   <td>Page type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sharing Panel
   </td>
   <td><strong>Bounce Rate</strong>
   </td>
   <td>% 
<p>
E.g. 19.3%
   </td>
   <td># of FE sessions when users exit the page without clicking anything except close button / total # of FE sessions
<p>
(only for page with total session cnt > 1000)
   </td>
   <td>Region
   </td>
   <td>Page type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sharing Panel
   </td>
   <td><strong>freq_cnt</strong> 
   </td>
   <td>Int
<p>
E.g. 2M
   </td>
   <td># of share event
   </td>
   <td>Region
   </td>
   <td>sharing_option (data field)
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sharing Panel
   </td>
   <td><strong>user_cnt</strong>
   </td>
   <td>Int
<p>
E.g. 2M
   </td>
   <td># of users who share the options
   </td>
   <td>Region
   </td>
   <td>sharing_option (data field)
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sharing Panel
   </td>
   <td><strong>CTR</strong>
   </td>
   <td>%
<p>
E.g. 13.1%
   </td>
   <td># of FE events when users click the share button / # of FE events when users impress on the share button
   </td>
   <td>Region
   </td>
   <td>sharing_option (data field)
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td rowspan="4" >Chat dashboard 
<p>
<a href="https://datasuite.shopee.io/dashboard/dashboard/9defde8b-3a51-4a3d-9fac-20e85186365e/normal?page=0">https://datasuite.shopee.io/dashboard/dashboard/9defde8b-3a51-4a3d-9fac-20e85186365e/normal?page=0</a>
   </td>
   <td>Chat
   </td>
   <td><strong>Chat DAU</strong>
   </td>
   <td>Int
<p>
E.g. 19M
   </td>
   <td>no. of users who visited chat page daily
   </td>
   <td>Region
<p>
Platform
   </td>
   <td>User Type
<p>
(Buyer/Seller)
   </td>
   <td>Daily
<p>
Weekly
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Chat
   </td>
   <td><strong>Login rate</strong> 
   </td>
   <td>%
<p>
E.g. 23.17%
   </td>
   <td>total users who visited chat page / total shopee users
   </td>
   <td>Region
<p>
Platform
   </td>
   <td>User Type
<p>
(Buyer/Seller)
   </td>
   <td>Daily
<p>
Weekly
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Chat
   </td>
   <td><strong>Chatroom conversion rate</strong>
   </td>
   <td>%
<p>
E.g. 79.41%
   </td>
   <td>total users who entered chatroom page / total users who visited chat page
   </td>
   <td>Region
<p>
Platform
   </td>
   <td>User Type
<p>
(Buyer/Seller)
   </td>
   <td>Daily
<p>
Weekly
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Chat
   </td>
   <td><strong>Active convo cnt</strong>
   </td>
   <td>Int
<p>
E.g. 135M
   </td>
   <td>total no.of active conversations in a week
   </td>
   <td>Region
<p>
Platform
   </td>
   <td>Conversation type 
<ul>

<li>B2B : buyer-to-buyer

<li>B2S : buyer-to-seller

<li>S2S : seller-to-seller

<p>
Entry point
<ul>

<li>Product

<li>Direct chat
</li>
</ul>
</li>
</ul>
   </td>
   <td>Daily
<p>
Weekly
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td rowspan="14" >Account Key Metrics Dashboard
<p>
<a href="https://datasuite.shopee.io/dashboard/dashboard/abd5e8b9-1fb0-4c22-bfd2-960a350124bc/normal?page=0">https://datasuite.shopee.io/dashboard/dashboard/abd5e8b9-1fb0-4c22-bfd2-960a350124bc/normal?page=0</a>
   </td>
   <td>Sign Up&Login Metrics
   </td>
   <td>DAU
   </td>
   <td>Int
<p>
E.g. 5.2M
   </td>
   <td>Daily Active Users
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sign Up&Login Metrics
   </td>
   <td>New Users
   </td>
   <td>Int
<p>
E.g. 193K
   </td>
   <td>Daily Registered New Users (Action Sign Up Success)
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sign Up&Login Metrics
   </td>
   <td>New Users Pct
   </td>
   <td>%
<p>
E.g. 0.3%
   </td>
   <td>Daily Registered New Users (Action Sign Up Success)/Daily Active Users
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sign Up&Login Metrics
   </td>
   <td>DAD 
   </td>
   <td>Int
<p>
E.g. 16M
   </td>
   <td>Daily Active Devices
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sign Up&Login Metrics
   </td>
   <td>Logged In Pct
   </td>
   <td>%
<p>
E.g. 80.9%
   </td>
   <td>Logged in devices / Total of devices that opened for that specific day
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sign Up & Login User Funnel
   </td>
   <td>User Enter Sign Up/ Login Flow
   </td>
   <td>Int
<p>
E.g. 87K
   </td>
   <td>Users who clicked the buttons related to "sign up/login" in the "sign up/ login" page
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sign Up & Login User Funnel
   </td>
   <td>User Switch To login/Sign Up flow
   </td>
   <td>%
<p>
E.g. 22.4%
   </td>
   <td>User who clicked sign up at login page or clicked sign up at login page
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sign Up & Login User Funnel
   </td>
   <td>Dropout Rate
   </td>
   <td>%
<p>
E.g. 22.4%
   </td>
   <td>Users who clicked non-signup related or non-login related
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Sign Up & Login User Funnel
   </td>
   <td>Sign Up/ Login Conversion
   </td>
   <td>%
<p>
E.g. 45.6%
   </td>
   <td>Successfully login/sign up users / Users who have entered the signup/ login flow
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>OTP
   </td>
   <td>OTP Success Rate
   </td>
   <td>%
<p>
E.g. 10%
   </td>
   <td>Success Verified OTP/ Total OTP per flow (tracking_id)
   </td>
   <td>Region
   </td>
   <td>OTP Type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Account Management
   </td>
   <td>Email verified rate
   </td>
   <td>%
<p>
E.g. 10%
   </td>
   <td>Users who have verified email address/ Total Users
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Account Management
   </td>
   <td>Email bind rate
   </td>
   <td>Number
<p>
E.g. 10%
   </td>
   <td>User who have binded their email address/ Total Users
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Account Management
   </td>
   <td>Phone verified rate 
   </td>
   <td>%
<p>
E.g. 10%
   </td>
   <td>Users who have verified phone number/ Total Users
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Account Management
   </td>
   <td>Set Password rate
   </td>
   <td>%
<p>
E.g. 10%
   </td>
   <td>Users who have set password/ Total Users
   </td>
   <td>Region
   </td>
   <td>
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td rowspan="11" >Search dashboard
<p>
Search Redash dashboard
   </td>
   <td>S&R
   </td>
   <td>Impression count
   </td>
   <td>Int
<p>
E.g. 10M
   </td>
   <td>Number of impression record
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>Impression user count
   </td>
   <td>Int
<p>
E.g 10M
   </td>
   <td>Number of impression users
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>Click count
   </td>
   <td>Int
<p>
E.g. 10M
   </td>
   <td>Number of click record
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>Click user count
   </td>
   <td>Int
<p>
E.g. 10M
   </td>
   <td>Number of click user
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>View count
   </td>
   <td>Int
<p>
E.g. 10M
   </td>
   <td>Number of view record
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>View User count
   </td>
   <td>Int 
<p>
E.g 10M
   </td>
   <td>Number of view user
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>Impression per user
   </td>
   <td>Float
<p>
E.g. 10.11%
   </td>
   <td>Number of impression per user
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>Click per user
   </td>
   <td>Float
<p>
E.g. 10.11%
   </td>
   <td>Number of click per user
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>View per user
   </td>
   <td>Float
<p>
E.g. 10.11%
   </td>
   <td>Number of view per user
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>CTR - count
   </td>
   <td>% 
<p>
E.g. 5%
   </td>
   <td>Click/Impression count
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>S&R
   </td>
   <td>CTR - user
   </td>
   <td>% 
<p>
E.g. 5%
   </td>
   <td>Click/Impression user
   </td>
   <td>Region
<p>
Platform 
<p>
App version
   </td>
   <td>Page_type
<p>
Page_section
<p>
Target_type
   </td>
   <td>Daily
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td rowspan="2" >Duration per user
<p>
<a href="https://confluence.shopee.io/download/attachments/1361216554/Feature_User_Duration_Weekly_Report_2022-05-02_to_2022-05-08.xlsx?version=1&modificationDate=1662538116000&api=v2">Feature_User_Duration_Weekly_Report_2022-05-02_to_2022-05-08.xlsx</a>
   </td>
   <td>Page duration
   </td>
   <td>Minute spent per user
   </td>
   <td>Float
<p>
E.g. 
   </td>
   <td>Minute spent per user
   </td>
   <td>Region
   </td>
   <td>Page_type
   </td>
   <td>Weekly
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>Page duration
   </td>
   <td>% of total time spent in app
   </td>
   <td>%
<p>
E.g. 10%
   </td>
   <td>Minute spent/Total minute spent in app
   </td>
   <td>Region
   </td>
   <td>Page_type
   </td>
   <td>Weekly
   </td>
   <td>
   </td>
  </tr>
</table>


<h3>App release schedule</h3>


Including hot fix: [https://docs.google.com/spreadsheets/d/1YBTTqUPM06s1S1Q8KgT90RRi-1BGnlgbadtCA11fS3w/edit#gid=1481497808](https://docs.google.com/spreadsheets/d/1YBTTqUPM06s1S1Q8KgT90RRi-1BGnlgbadtCA11fS3w/edit#gid=1481497808)

Excluding hot fix: [https://calendar.google.com/calendar/embed?src=c_pqe7vjbl2vi6spbftgmb80iato%40group.calendar.google.com&ctz=Asia%2FSingapore](https://calendar.google.com/calendar/embed?src=c_pqe7vjbl2vi6spbftgmb80iato%40group.calendar.google.com&ctz=Asia%2FSingapore) and [https://calendar.google.com/calendar/embed?src=c_oehulj798vdkkv8pc9je4gq1oo%40group.calendar.google.com&ctz=Asia%2FSingapore](https://calendar.google.com/calendar/embed?src=c_oehulj798vdkkv8pc9je4gq1oo%40group.calendar.google.com&ctz=Asia%2FSingapore)

<h3>Web release schedule</h3>


Regular release: Every Thurs

Hotfix/Adhocs: 

PIC ash.angsh

<h3>Campaign date</h3>


[https://docs.google.com/spreadsheets/d/1hSa8hjTImVnctWmLzmfonVVyQ3G6uv4n9URqoGhJV0w/edit#gid=1648881842](https://docs.google.com/spreadsheets/d/1hSa8hjTImVnctWmLzmfonVVyQ3G6uv4n9URqoGhJV0w/edit#gid=1648881842) 

TheH channel "Shopee Campaign Noti" will send out all the campaigns of all regions which was planed at the beginning of the year .

<h3>Holiday</h3>


Add holiday for each region

<h3>Known tracking issue</h3>


[https://docs.google.com/spreadsheets/d/1C6bSShxXDkk1oYP03kCBBYOxIzqwXxWYcwKUFsEB2ro/edit#gid=1401675639](https://docs.google.com/spreadsheets/d/1C6bSShxXDkk1oYP03kCBBYOxIzqwXxWYcwKUFsEB2ro/edit#gid=1401675639)

<h3>Checking Accuracy level </h3>


<h4>**30 Mar 2023** </h4>


6 Insight → Precision rate ~70% 



* 4 confirmed to be tracking issue (impression did not send - P0 tracking): [https://jira.shopee.io/projects/SPTSS/queues/issue/SPTSS-603](https://jira.shopee.io/projects/SPTSS/queues/issue/SPTSS-603)
* 1 abnormal increase for click (FPM is checking): [https://trafficsuite.shopee.io/we/we_monitor/trafficInsight/all/a7e9c5950ad24a739f6eb5d72d5b61f7](https://trafficsuite.shopee.io/tms/tms_monitor/trafficInsight/all/a7e9c5950ad24a739f6eb5d72d5b61f7)
* 1 abnormal increase for view (FPM is checking): [https://trafficsuite.shopee.io/we/we_monitor/trafficInsight/all/19887eeec8264b74af9f5c58078c4f13](https://trafficsuite.shopee.io/tms/tms_monitor/trafficInsight/all/19887eeec8264b74af9f5c58078c4f13)

<h2>Reference</h2>


Microsoft PowerBI: [https://docs.microsoft.com/en-us/power-bi/create-reports/insights](https://docs.microsoft.com/en-us/power-bi/create-reports/insights)

Google Analytics: [https://support.google.com/analytics/answer/9443595?hl=en&utm_id=ad#delay](https://support.google.com/analytics/answer/9443595?hl=en&utm_id=ad#delay)

Redash: Redash S&R (Pls apply access via SOUP for Redash)

(Shopee) YMAL troubleshoot

Meituan: [https://tech.meituan.com/2017/04/21/order-holtwinter.html](https://tech.meituan.com/2017/04/21/order-holtwinter.html)

Alert message (should display Absolute or Relative change): [https://dataschool.com/misrepresenting-data/relative-vs-absolute-change/](https://dataschool.com/misrepresenting-data/relative-vs-absolute-change/)
