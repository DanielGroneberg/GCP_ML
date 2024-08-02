We are attempting to predict which customers might buy products from an ecommerce site once they return, First, the lab has us walk through some SQL commands. There are two features to consider: time spent on the site and whether they _bounced_ (immediately left. The lab points out that a potential issue is that these features are highly correlated since they're both related to time spent on the site.
One of the first things we do is query a table with these features, and our target, which is _will_buy_on_return_:

![image](https://github.com/user-attachments/assets/cfa32a3a-467b-4f9b-b38f-e1a35989ca15)


### Creating a dataset to store ML models

![image](https://github.com/user-attachments/assets/03c014fe-b4c6-4267-9a4b-f18e89cd4280)

### Deciding on the right type of model:

Our task is to classify users based on whether they are likely to make a purchase on returning to the site. I incorrectly thought of this as a forecasting problem since we're dealing with a future event, but the forecasting the question is refering to here is more along the lines of forecasting future humidity etc. i.e. a regression task.

![image](https://github.com/user-attachments/assets/5ea0c526-5345-462c-9c0f-f3dd831081fb)
