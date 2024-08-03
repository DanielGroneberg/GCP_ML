Basically, Google has options available for every experience level. I'm not sure what preconfigured models look like or how one would use them, but I've already used the other two types: we just coded out some basic logistic regressor and XGBoost models using SQL in BigQuery and I've built all sorts of ML models with Tensorflow/Keras in Colab.

![image](https://github.com/user-attachments/assets/dfb9603c-50be-47d1-be5f-c66e2b94636a)

Drawbacks of each:

Pre-trained APis, BigQuery, AutoML: [Only certain types of models are available](https://cloud.google.com/bigquery/docs/bqml-introduction).

The way they ordered things in the slide below is weird. BigQuery requires SQL coding, and AutoML doesn't - you build models with a point and click GUI. Whatever.

![image](https://github.com/user-attachments/assets/0e6b6986-2cb4-4b6e-a9c0-a324ee0afe06)

### Useful comparison between products

This sums it up. That said, they say you can't create ML audio models with a DIY approach, but I'm pretty sure that's not true. Someone in my bootcamp used Tensorflow [to create an ML model with audio](https://github.com/benjmcpeek/Audio-Classification-Model-Major-Minor/blob/main/code/03_final_models.ipynb)

![image](https://github.com/user-attachments/assets/93055d41-37fc-41fd-8ee0-2de8bdde9510)