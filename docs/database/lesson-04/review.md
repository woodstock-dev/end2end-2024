# Lesson 4 Review

## Homework

Based on the work we've done above, create a customer review table
that allows the customer to rate a track they have
purchased on a scale of 1 to 5 and leave a short comment.

> * What are the risks of this type of review?
> * How can you mitigate those risks?

### Creating a customer review table

```sql
CREATE TABLE track_reviews(
    id SERIAL PRIMARY KEY NOT NULL,
    customer_id int NOT NULL,
    track_id int NOT NULL,
    rating int NOT NULL DEFAULT 3,
    comments varchar(512)
);

-- Create a function to see if the customer exists
-- Errata - Subselects ARE NOT allowed in PGSQL for check constraints
CREATE FUNCTION customer_exists (id int) RETURNS BOOLEAN
AS $$
BEGIN
    RETURN EXISTS (SELECT * FROM customer WHERE customer_id = _id);
END;
$$ LANGUAGE PLPGSQL

-- Ensure the customer exists at the time of insert.
ALTER TABLE track_reviews 
ADD CONSTRAINT chk_track_customer
CHECK (customer_exists(customer_id));

-- Ensure the rating is greater than or equal to 1 and less than or equal to 5.
ALTER TABLE track_reviews
ADD CONSTRAINT chk_track_rating
CHECK (rating >= 1 and rating <=5);

-- Ensure the track exists, and will continue to exist
ALTER TABLE track_reviews 
ADD CONSTRAINT fk_track_reviews_track 
FOREIGN KEY (track_id) REFERENCES track(track_id);
```

Discussion Points

Free form text can be a little risky, you may want to add an approved column
or an automated review process to ensure no foul langage or inappropriate slang
is used.