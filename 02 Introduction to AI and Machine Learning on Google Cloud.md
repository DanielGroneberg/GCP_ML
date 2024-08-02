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
