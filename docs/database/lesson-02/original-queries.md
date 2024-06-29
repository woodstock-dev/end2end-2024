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
# Structured Query Language (SQL)

## Simple Queries
```sql
-- Get some data, from one or more tables, and filter on the following filters
-- filters are predicates that define the scope of data and or the behavior
-- of how data is returned

-- A query to return all data (no rules);
select artist_id, name from artist;

-- A query to return an artist by a specfic name:
select artist_id, name from artist where name = 'Aerosmith';

-- A query to return all artists who's name starts with the letter 'A'
select artist_id, name from artist where name like 'A%';

-- A query to return all artists who's name starts with 'A' or 'a'
select artist_id, name from artist where LOWER(name) like 'a%';
```

## Joining Data

```sql
--- Find all of the albums an artist has created

--- An inner join  (only where values match)
select a.name, b.title from artist a inner join album b on a.artist_id = b.artist_id;

--- An left outer join (includes null matching)
select a.name, b.title from artist a left join album b on a.artist_id = b.artist_id;
```

## Aggregating queries

Sometimes you want to run computations on your data and have the server
do the work. Afterall, that's what it's built to do.

```sql
--- Get the total number of albums an artist has created
select a.name, count(b.artist_id) from artist a 
	join album b on a.artist_id = b.artist_id 
	group by (a.name, b.artist_id);
```

## Aggregation and Filters

```sql
--- Get the total number of albums by artist and list from most to least
select a.name, count(b.artist_id) as album_count from artist a 
	join album b on a.artist_id = b.artist_id 
	group by (a.name, b.artist_id) order by album_count desc;
---