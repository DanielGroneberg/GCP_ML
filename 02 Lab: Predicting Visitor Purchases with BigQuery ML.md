We are attempting to predict which customers might buy products from an ecommerce site once they return, First, the lab has us walk through some SQL commands. There are two features to consider: time spent on the site and whether they _bounced_ (immediately left. The lab points out that a potential issue is that these features are highly correlated since they're both related to time spent on the site.
One of the first things we do is query a table with these features, and our target, which is _will_buy_on_return_:

![image](https://github.com/user-attachments/assets/cfa32a3a-467b-4f9b-b38f-e1a35989ca15)


### Creating a dataset to store ML models

![image](https://github.com/user-attachments/assets/03c014fe-b4c6-4267-9a4b-f18e89cd4280)

### Deciding on the right type of model:

Our task is to classify users based on whether they are likely to make a purchase on returning to the site. I incorrectly thought of this as a forecasting problem since we're dealing with a future event, but the forecasting the question is refering to here is more along the lines of forecasting future humidity etc. i.e. a regression task.

![image](https://github.com/user-attachments/assets/5ea0c526-5345-462c-9c0f-f3dd831081fb)

### Running the classification model

We are given some SQL code to run the model

```sql
CREATE OR REPLACE MODEL `ecommerce.classification_model`
OPTIONS
(
model_type='logistic_reg',
labels = ['will_buy_on_return_visit']
)
AS

#standardSQL
SELECT
  * EXCEPT(fullVisitorId)
FROM

  # features
  (SELECT
    fullVisitorId,
    IFNULL(totals.bounces, 0) AS bounces,
    IFNULL(totals.timeOnSite, 0) AS time_on_site
  FROM
    `data-to-insights.ecommerce.web_analytics`
  WHERE
    totals.newVisits = 1
    AND date BETWEEN '20160801' AND '20170430') # train on first 9 months
  JOIN
  (SELECT
    fullvisitorid,
    IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM
      `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid)
  USING (fullVisitorId)
;
```
And we get a message informing us that the model has run successfully:

![image](https://github.com/user-attachments/assets/35b4d1f3-a714-44a6-9f9c-2ad44713bb32)

I have to say, this is quite clean looking and easy to understand:

![image](https://github.com/user-attachments/assets/12cf41b6-abbf-4def-903d-5866b28ef52d)

### Evaluating the performance of our classification model

The lecture states that, "For classification problems in ML, you want to minimize the False Positive Rate (predict that the user will return and purchase and they don't) and maximize the True Positive Rate (predict that the user will return and purchase and they do)." Although it's probably universally true that minimizing false positive and maximizing true positive is preferable, there will pretty much always be a tradeoff, and in certain cases, like COVID tests, false positives are probably not as harmful as false negatives, which is basically the opposite of our situation here. 

Anyway, we can assess model performance using a ROC curve.

![image](https://github.com/user-attachments/assets/696f0ce9-6832-4f84-ae90-48c42b7c6eb1)

As we understood before, the two features we picked are highly correlated, since both are related to time spent on the site. In order to expand the model's explanatory ability, we need to add new features, including:

`How far the visitor got in the checkout process on their first visit (6 = Completed Purchase)
Where the visitor came from (traffic source: organic search, referring site etc.)
Device category (mobile, tablet, desktop)
Geographic information (country)`

### Training the new model

![image](https://github.com/user-attachments/assets/962ca3c3-78c3-461e-aab5-c65c1aea2c04)

### Evaluating the new model

![image](https://github.com/user-attachments/assets/81044292-2865-408b-85cc-fff8fa47954d)

Just adding a few new features made the model perfom significantly better.

### Making predictions

"The predictions are made in the last 1 month (out of 12 months) of the dataset."
Our train set was the first 9 months and the test the final 3, meaning we are making predictions using the train dataset, i.e. data the model has not directly seen.

```sql
SELECT
*
FROM
  ml.PREDICT(MODEL `ecommerce.classification_model_2`,
   (

WITH all_visitor_stats AS (
SELECT
  fullvisitorid,
  IF(COUNTIF(totals.transactions > 0 AND totals.newVisits IS NULL) > 0, 1, 0) AS will_buy_on_return_visit
  FROM `data-to-insights.ecommerce.web_analytics`
  GROUP BY fullvisitorid
)

  SELECT
      CONCAT(fullvisitorid, '-',CAST(visitId AS STRING)) AS unique_session_id,

      # labels
      will_buy_on_return_visit,

      MAX(CAST(h.eCommerceAction.action_type AS INT64)) AS latest_ecommerce_progress,

      # behavior on the site
      IFNULL(totals.bounces, 0) AS bounces,
      IFNULL(totals.timeOnSite, 0) AS time_on_site,
      totals.pageviews,

      # where the visitor came from
      trafficSource.source,
      trafficSource.medium,
      channelGrouping,

      # mobile or desktop
      device.deviceCategory,

      # geographic
      IFNULL(geoNetwork.country, "") AS country

  FROM `data-to-insights.ecommerce.web_analytics`,
     UNNEST(hits) AS h

    JOIN all_visitor_stats USING(fullvisitorid)

  WHERE
    # only predict for new visits
    totals.newVisits = 1
    AND date BETWEEN '20170701' AND '20170801' # test 1 month

  GROUP BY
  unique_session_id,
  will_buy_on_return_visit,
  bounces,
  time_on_site,
  totals.pageviews,
  trafficSource.source,
  trafficSource.medium,
  channelGrouping,
  device.deviceCategory,
  country
)

)

ORDER BY
  predicted_will_buy_on_return_visit DESC;
```

Each of the commerce sessions which took place in July (the final month) have been given three new fields:

`predicted_will_buy_on_return_visit: whether the model thinks the visitor will buy later (1 = yes)
predicted_will_buy_on_return_visit_probs.label: the binary classifier for yes / no
predicted_will_buy_on_return_visit_probs.prob: the confidence the model has in it's prediction (1 = 100%)`

In simpler terms, there is now a (slightly redundant) probability given for whether each user will/will not return to complete their order.
