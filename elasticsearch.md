## Problems with Elasticsearch
- It's basically a search Engine. So it's optimized for searching. It requires more disk space and ram to hold additional helper
data structures to perform searches quickly.
- It holds a lot of index and auxiliary data structures in Ram for better search speed. So if a instance goes down, it will require
more time to ramp up those data structures back into the Ram. For example, it may have to create a TF-IDF data structure
from the raw data from the hard disk. Thus overall respone time is long.
