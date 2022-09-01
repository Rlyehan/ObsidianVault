222022-09-01

Style: #ArticleNotes 

Domain: #DataOps 

Tags:

# Ensuring Data Quality, With Great Expectations

What data quality exactly means is something that we need to define basedn on the project requirements. 

Ensuring that oru data is in lien with these quality metrics we defined is massivly important. bad data can lead to wrong decisions, causing a lot of damage.

And with the rise of applications making use of ML this becomes even more important, since the issue might remain hidden even longer.

we will make use of [[Great Expectations]] here to show how to validate data quality. We will be using an postgres DB, as such we need to get it up and running in an docker container:

```bash
docker run --name pg_local -p 5432:5432 -e POSTGRES_USER=sde -e POSTGRES_PASSWORD=password -e POSTGRES_DB=data_quality -d postgres:12.2
```

```bash
pgcli -h localhost -U sde -p 5432 -d data_quality
```

In there we create a simple table:
```sql
CREATE SCHEMA app;

CREATE TABLE IF NOT EXISTS app.order(
    order_id varchar(10),
    customer_order_id varchar(15),
    order_status varchar(20),
    order_purchase_timestamp timestamp
);

INSERT INTO app.order(order_id, customer_order_id, order_status, order_purchase_timestamp)
VALUES ('order_1','customer_1','delivered','2020-07-01 10:56:33');

INSERT INTO app.order(order_id, customer_order_id, order_status, order_purchase_timestamp)
VALUES ('order_2','customer_1','delivered','2020-07-02 20:41:37');

INSERT INTO app.order(order_id, customer_order_id, order_status, order_purchase_timestamp)
VALUES ('order_3','customer_2','shipped','2020-07-03 11:33:07');
```

### Defining Test cases

As with all testing, we want to define our test cases, or expecations as they are called in this context.

so first we want to make an new folder in wich to create our expectations in and initialize a new great_expectations instance.
```bash
mkdir data_quality
great_expectations init -d data_quality
# OK to proceed? [Y/n]: Y
# Would you like to configure a Datasource? [Y/n]: Y
# choose SQL, Postgres,
# [my_postgres_db]: data_quality
# enter the configuration from the docker command above

```

and then create a new suite:
```bash
great_expectations suite new
```

Great Expectations will have a look at the data we gave to it and generate an example suite. To make it easier for us to understand them, we open the data docs site that was generated.

When we open the automatically created expectations there, they are shown to us in an UI that helps us understand them.

```bash
great_expectations suite new
# lets call it error
```

```bash
great_expectations suite edit app.order.error
```

We then remove all the auto generated test cases and make a simple one like so:
![Jupyter nb edit app.order.error 1](https://www.startdataengineering.com/images/data_quality_ge/expectation_jp_1.png)
![[Pasted image 20220901111052.png]]

After saving, we can look at it on the data docs site to verify we made it correctly.

## Running your test cases (aka checkpoint)
Now that we ahve a test we want to execute it of course. To so we first need to create a checkpoint. Checkpoints are a set of expectations that we want to execute whenever we call it.
```bash
great_expectations checkpoint new first_checkpoint app.order.error
```

and then run it:

```bash
great_expectations checkpoint run first_checkpoint
```

Now we might want to verify that the expectation works as we expect. We can do this for example by adding new data to our table that ahas unexpected contetn in the costumer_id column we are veriying.

If we do this and the xpectations fails the next time we execute it, we verified that it failed as expected, assuring us that it validates what we want.

### Change output storage location

AS we have it configured now, we are storing the output in our local repository. Chances are taht we would like to store them elsewhere isntead, so that for example we can use the results for our observability.

To change where we store our results, we need to modify the  `great_expectations.yml` file and add a new `store` to the `stores:` section. We can add our postgres db as an example:
```diff

stores:


  validations_store:
    class_name: ValidationsStore
    store_backend:
      class_name: TupleFilesystemStoreBackend
      base_directory: uncommitted/validations/
+ validations_postgres_store:
+   class_name: ValidationsStore
+   store_backend:
+     class_name: DatabaseStoreBackend
+     credentials: ${data_quality}


-validations_store_name: validations_store
+validations_store_name: validations_postgres_store
```

and then validate it by calling:
```bash
great_expectations store list
```


### Scheduling
Now that we have our suite in place, we can easily schedule its execution with a `bash` or `python` script that we can add to most orchistration services.
 
___
# References
[Ensuring Data Quality, With Great Expectations · Start Data Engineering](https://www.startdataengineering.com/post/ensuring-data-quality-with-great-expectations/)