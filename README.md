# MySQL


# What is Database Indexing (索引)?

- Improve Query Performance
  - Searching for a value is fast
  - Logarithmic scalability

一個很好的比喻是電話簿，通常電話簿是依照 lastName 來排序，所以我們今天如果要使用 lastName 來查詢的時候會很快，因為早已排序好，但是如果我們今天要使用 firstName 來查詢，那就只能說抱歉了資料庫會一個一個查。所以這種情況我們也可以說電話簿是依照 lastName 在 Index的。


- An Index is an ordered representation of the indexed data. (想想 binary search 就是透過 sorted array 在快速查找)

Indexing is sort of like telling the table that this is an important column . 
It can create a tree structure (B-Tree : Balanced Tree , the leave nodes will always be at the same level , this way we can guarantee that we can take the same number of steps to find the exact node) , which will increase the speed of searching through the table.
A much preferred way is to start indexing when we are building the schema , but we can still add indexes after the table has been created , using the ALTER keyword.

![image](https://user-images.githubusercontent.com/61279365/157586984-16185efb-f844-45f2-a3f1-8f6f28f3bfde.png)


索引在資料庫裡面扮演了很重要的角色，如果沒有 Index ，資料庫將會從頭掃描到尾，一直到找尋到符合目標為止，一旦 Table 裡面的資料量增加到一定的 size 後，搜尋的速度就會愈慢，效能就會愈差，所以一個好的 Table 要有相對應的索引來幫助搜尋。


!! Indexing makes reading faster but writing slower. 所以千萬不是 Indexing 愈多愈好

這個 Doubly Linked-List 的概念非常重要，當我們的 Database 在搜尋到每一個 tree node 時，會是透過這樣的方式去找到隔壁的 node ，不然會變成需要重投來過一遍 ( This is not efficient at all!)
![image](https://user-images.githubusercontent.com/61279365/157588680-5fec4ee3-3664-435f-bcb9-41e376fbd8f7.png)


我們用下面這張 Table 來實際了解一下我們在 Indexing 時實際上做了甚麼
![image](https://user-images.githubusercontent.com/61279365/157588987-fe1bc2a3-1327-43a6-8d22-ffec11186441.png)

Ex.1 Name Index
![image](https://user-images.githubusercontent.com/61279365/157589112-2d36fdec-6c5b-4d7c-9d06-f9801cc95d38.png)

The tree only contains the values that you put index on.

## Types of Indexing 
1. Primary Key : Most commonly seen ，而且一個 Table 通常只能有一個 Primary Key (Cannot have duplicates & Cannot store "NULL" value.  
2. Unique Index : Cannot have duplicates , but can store NULL value.
3. Non-unique Index
4. Full-Text search : Can only be used in type of CHAR , VARCHAR , TEXT 



我們可以先來了解一下 MySQL Database 是怎麼找資料的
```
EXPLAIN SELECT * FROM users WHERE id = 1;
```
![image](https://user-images.githubusercontent.com/61279365/157596294-96bd945c-ed0e-4668-923c-f483447ec555.png)



CONST / EQ_REF

- Peforms a B-Tree Traversal to find a single value
- Basically a binary search


## Methods for Indexing

```
CREATE TABLE products(
  product_id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  product_name VARCHAR(100) NOT NULL,
  product_category VARCHAR(50),
  product_price DECIMAL(10,2) NOT NULL,
  product_sku CHAR(10) NOT NULL,
  product_decription VARCHAR(500),
  PRIMARY KEY (product_id),
  INDEX idx_names (product_name,product_category)
)

```
![image](https://user-images.githubusercontent.com/61279365/157584739-a8aaaa58-8048-430a-89ae-cb29b81c218a.png)



因為 SKU 一定是唯一值，所以很好做為 index 的標的
```

ALTER TABLE products
CREATE INDEX idx_sku
ADD UNIQUE idx_sku (product_sku);

```


## Removing Indexes

```
DROP INDEX idx_sku ON products;
DROP INDEX (column_name) ON (;

```


假設建立兩個索引 index_1 (sex, name, age) 跟 index_2 (name, age, sex)，在 index_1 中可以發現最左(最先)判斷的性別，但性別包含過多的資料，導致搜尋出來的結果還是很多，並無太大幫助，所以最左邊的應該放置更有識別性的欄位，像是 index_2的 name 較具有唯一性，才能大大提升搜尋的效率。


MySQL資料庫提供「 EXMPLIN 」指令，讓我們分析一個查詢敘述。在要使用的 SQL 語句最前面加上 " EXPLAIN "，用來判斷先前所建立的索引有沒有如預期中使用，

```
EXPLAIN SELECT * FROM member WHERE name = 'Michael';
```
