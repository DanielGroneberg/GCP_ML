### Data types
* Unstructured: documents, images, audio files
* Structured: tables

### Storage types:
Depending on how frequently the data needs to be accessed, you should choose an appropriate storage type. Archival is the cheapest, but must be stored for at least a year at the minimum:

![image](https://github.com/user-attachments/assets/d394da8b-814c-44d7-bffe-aeaf67337938)

### Structured data and relevant products
SQL is ideal for structured data: it's literally the _Structured Querry Language!_ I did some basic research, and it sounds like transactional workloads are more along the lines of continual flows of data e.g. continual sales taking place around the world. It's for streams of data.

Analytical workloads would be more like querrying potentially very large batches of data for analysis, as the name implies. It's possible that analytical workflows would be querrying much larger amounts of data in a given querry, since transactional is dealing with a more-or-less continual flow vs large fetching operations all at once.

I also looked into SQL vs NoSQL a bit: it sounds like NoSQL is just a different storage paradigm which can use JSON etc. for structuring. Not sure what the advantage of one or the other is yet, but I'm sure it'll come up.

![image](https://github.com/user-attachments/assets/6ed6c06e-8733-4fec-b5de-51818953a72f)

## Machine learning basics:
### Supervised vs unsupervised learning:

Supervised tasks are generally more straightforward to score or assess performance: if the goal is to predict house prices given a variety of features (# of rooms, location, etc.) it's easy to break these data into train/test sets, develop a model using the training set and score against the test set. Unsupervised tasks are more along the lines of pattern recognition within the data. With the same housing dataset, an unsupervised task might be deciding where to build a new grocery store based on geographic density, land values, etc. This would be an unsupervised task since there isn't a clear way to assess performance until after the store is built, and there is no pre-existing (labeled) feature for store revenue, etc.

![image](https://github.com/user-attachments/assets/6685d569-8ca8-48b8-8d62-26d966787e03)
