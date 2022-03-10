# MySQL


# What is Indexing (索引)?

索引在資料庫裡面扮演了很重要的角色，如果沒有 Index ，資料庫將會從頭掃描到尾，一直到找尋到符合目標為止，一旦 Table 裡面的資料量增加到一定的 size 後，搜尋的速度就會愈慢，效能就會愈差，所以一個好的 Table 要有相對應的索引來幫助搜尋。

## Types of Indexing 
1. Primary Key : Most commonly seen ，而且一個 Table 通常只能有一個 Primary Key (Cannot have duplicates & Cannot store "NULL" value.  
2. Unique Index : Cannot have duplicates , but can store NULL value.
3. Non-unique Index
4. Full-Text search
