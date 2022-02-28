## Assumptions

- The system aims for small concurrency and no need of a distributed architecture
- Only support at most 8 char long url, notice that I am using base62 ([a-zA-Z0-9]), that means it can at most support 8^62 urls, which is enough for most cases.
- This is supposed to be deployed within a containeration system (such as k8s), we can afford to have multiple instances for robustness, thus no need to make the application itself into a cluster mode(nodejs cluster).
- To avoid one to many mapping (sort of storage wasting) of a long url, the system use an LRU cache to ease this pain, namely if a same long url is frequently been submitted to generate a short one, the system will return the already generated cache instead of generating a new short url again.
- Assume the longest url will be 2048, this should cover most of the cases.
- Don't support customized short url (user given short url), if need this, one easy way is to add one `short_url` column to the table and also stores the shortUrls instead of computing from ids, this way, we can index this column and also check against existence to support customized shortUrls.
