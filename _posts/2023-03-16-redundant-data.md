---
layout: post
title:  "Redundant Data on the Database"
date:   2023-03-16 21:19:17 -0300
smmry: "Are redundant data on the database always a bad practice? The answers is the same as all import engineering matters: it depends"
read_time: 4 min
---

People learn about [data normalization](https://en.wikipedia.org/wiki/Database_normalization) on formal courses and aim to reduce data duplication very early in their exposure to databases. However, in some cases, normalization can lead to complex queries and slow performance. This is where redundant data can come in handy. Redundant data, as the name suggests, involves storing the same data in multiple locations to improve query performance.

I have identified an opportunity to optimize the queries at the company I work for by utilizing redundant data to cache information from one table into another. However, in order to respect privacy policies and protect sensitive information, I will modify the descriptions of the tables and their purposes while presenting the overall concept.

Let's suppose you have two very large tables: products and orders, and an smaller accounts table as well. The basic schema can be something like:
```
orders {
  id: string;
  product_id: string;
  created_at: timestamp
}

products {
  id: string;
  created_at: timestamp
  account_id: string
}

accounts {
  id: string
}
```

What happens if you need to get the the accounts ordered by the date of the last order? And for the sake of the example, we can't add an `account_id` to the `orders` table. Well, we were using a query like:

```sql
SELECT * FROM accounts
JOIN products ON accounts.id = products.account_id
JOIN orders ON products.id = orders.product_id
WHERE EXISTS(
  SELECT 1
  FROM orders
  where orders.product_id = products.id
)
ORDER BY (
  SELECT MAX(orders.created_at)
  FROM orders
  WHERE orders.product_id = products.id
) DESC
LIMIT 100
```

Ok, this query isn't the most complex to write, but now there's a heavy join between `products` and `orders` and the only thing we want to know is what are the accounts order by the last product purchase. Of course there is a simpler solution, add `account_id` to the `orders` table, but I can't do that, it wouldn't make sense in the real scenario that I simplified into `accounts`, `products` and `orders`. Well, this is where the redundant data can aids us.

This query is found in a Ruby on Rails application. Rails has [Active Record Callbacks](https://guides.rubyonrails.org/active_record_callbacks.html#creating-an-object) that can be called on an event like `after_create`. It works like a SQL [trigger](https://www.postgresql.org/docs/current/sql-createtrigger.html).

So, the solution was to create a callback after the creation of an `order` that updated a new column on the `accounts` table. The new schema looks like this:

```
orders {
  id: string;
  product_id: string;
  created_at: timestamp
}

products {
  id: string;
  created_at: timestamp
  account_id: string
}

accounts {
  id: string
  last_order_at: timestamp
}
```

And now the query is simpler as well:

```sql
SELECT * FROM accounts
WHERE last_order_at IS NOT NULL
ORDER BY last_order_at DESC
```

There is only one caveat, I had to populate the `last_order_at` column on all accounts before changing the query. Even though the tables are large, it was not a complicated data migration.

# Conclusion

In certain situations, redundant data on an SQL database can be useful, and can improve performance by reducing the complexity of the queries needed to retrieve information. 
By doing so, you can avoid the need for a join when you want to retrieve the last order date for a particular account. Instead, you can simply query the table directly. This can be faster and more efficient, especially if you have a large number of records.

However, it is important to note that storing redundant data can also have downsides. For example, it can increase the storage requirements for the database and can make it more difficult to maintain data consistency, especially if the redundant data is not updated correctly. Imagine what could happen if the callback is poorly written, it could cause data to be wrongly consumed, for example sending emails telling that a payment was not received.