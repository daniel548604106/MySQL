# MySQL


# What is Indexing (索引)?
Indexing is sort of like telling the table that this is an important column . 
It can create a tree structure , which will increase the speed of searching through the table.
A much preferred way is to start indexing when we are building the schema , but we can still add indexes after the table has been created , using the ALTER keyword.

索引在資料庫裡面扮演了很重要的角色，如果沒有 Index ，資料庫將會從頭掃描到尾，一直到找尋到符合目標為止，一旦 Table 裡面的資料量增加到一定的 size 後，搜尋的速度就會愈慢，效能就會愈差，所以一個好的 Table 要有相對應的索引來幫助搜尋。

## Types of Indexing 
1. Primary Key : Most commonly seen ，而且一個 Table 通常只能有一個 Primary Key (Cannot have duplicates & Cannot store "NULL" value.  
2. Unique Index : Cannot have duplicates , but can store NULL value.
3. Non-unique Index
4. Full-Text search : Can only be used in type of CHAR , VARCHAR , TEXT 


## Methods for Indexing

```
CREATE TABLE products(
  product_id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  product_name VARCHAR(100) NOT NULL,
  product_category VARCHAR(50),
  product_price DECIMAL(10,2) NOT NULL,
  product_sku CHAR(10) NOT NULL,
  product_decription VARCHAR(500)
  PRIMARY KEY (product_id)
)

// or

CREATE TABLE product(
id,
name,
email,
PRIMARY KEY(id),
UNIQUE KEY(email)
)
```

```

CREATE INDEX email_index ON member(email);
ALTER TABLE member ADD INDEX(email;

```


## Removing Indexes

```
DROP INDEX email ON member;
DROP INDX 'PRIMARY' on member;

```


假設建立兩個索引 index_1 (sex, name, age) 跟 index_2 (name, age, sex)，在 index_1 中可以發現最左(最先)判斷的性別，但性別包含過多的資料，導致搜尋出來的結果還是很多，並無太大幫助，所以最左邊的應該放置更有識別性的欄位，像是 index_2的 name 較具有唯一性，才能大大提升搜尋的效率。


MySQL資料庫提供「 EXMPLIN 」指令，讓我們分析一個查詢敘述。在要使用的 SQL 語句最前面加上 " EXPLAIN "，用來判斷先前所建立的索引有沒有如預期中使用，

```
EXPLAIN SELECT * FROM member WHERE name = 'Michael';
```
