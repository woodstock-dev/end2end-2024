<!--
Copyright 2024 Ryan McGuinness

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

# SQL Queries

**Basic Queries**

1. **Get all artists:**
   ```sql
   SELECT * FROM artist;
   ```

2. **Get all albums:**
   ```sql
   SELECT * FROM album;
   ```

3. **Get specific artist details (e.g., artist with ID 2):**
   ```sql
   SELECT * FROM artist WHERE artist_id = 2;
   ```

4. **Get albums by a specific artist (e.g., artist with ID 1):**
   ```sql
   SELECT * FROM album WHERE artist_id = 1;
   ```

**Join Queries**

1. **Get artist name and album title together:**
   ```sql
   SELECT artist.name AS artist_name, album.title AS album_title
   FROM artist
   JOIN album ON artist.artist_id = album.artist_id;
   ```

**Aggregate Queries**

1. **Count the number of albums per artist:**
   ```sql
   SELECT artist.name AS artist_name, COUNT(album.album_id) AS num_albums
   FROM artist
   JOIN album ON artist.artist_id = album.artist_id
   GROUP BY artist.artist_id, artist.name; 
   ```
2. **Get the average album price per artist (if the column unit_price is not stored as a numeric datatype, you will need to cast it before averaging):**

```sql
SELECT artist.name AS artist_name, AVG(CAST(album.unit_price AS NUMERIC)) AS avg_price
FROM artist
JOIN album ON artist.artist_id = album.artist_id
GROUP BY artist.artist_id, artist.name; 
```
**Additional Notes**
* **More Complex Joins:**  You can involve other tables (like `genre` or `track`) to get more comprehensive results.
* **Filtering:** Use `WHERE` clauses within your join queries to narrow down your results (e.g., albums released after a certain year).
* **Sorting:** Use `ORDER BY` to sort your results (e.g., by artist name or number of albums). 

Feel free to ask if you'd like more examples or want to tailor these queries to specific tasks!