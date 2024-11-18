## Feedback 

In your submission.md table you've got some pretty substantial deviations from my totals. You don't need to fix the issues, but I would like you diagnose them. Calculate your records by year-month and my totals. Compare those and see if the issue becomes apparent. It looks like you might have just had one large file not upload. 

Nice work on this project, you can consider it complete. I'm going to read your files in order and give you feedback
as I move through them, from Task 1 to Task 3. 


### Task 1

* It'd be worth thinking a bit about how you'd do this without extracting all the files. That's useful in the case where the zip files fit on your machine but the unzipped files don't. 
* Put all your import statements in your first cell. This mid-file imports are a classic LLM tell.
*  It makes sense to use lines like `csv_files = glob.glob(f"{source_folder}/**/*.csv", recursive=True)` when you are working on a totally foreign system. Here, you're in total control, so I think it'd make much more sense to take advantage of the fact that this project is a one off on a machine you control. `os.listdir` is a lot simpler. 
*  Nice use of docstrings throughout. Given the size of your functions, I'd consider creating a `wedge_functions.py` file that you just import. 

It's a little weird to see this in a final submission: 


> BadRequest: 400 Error while reading data, error message: CSV processing encountered too many errors, giving up. Rows: 0; errors: 22; max bad: 0; error percent: 0; reason: invalid, message: Error while reading data, error message: CSV processing encountered too many errors, giving up. Rows: 0; errors: 22; max bad: 0; error percent: 0; reason: invalid, message: Error while reading data, error message: CSV table references column position 49, but line contains only 2 columns.; line_number: 2 byte_offset_to_start_of_line: 22 column_index: 49 column_name: "trans_id" column_type: DOUBLE; reason: invalid, message: Error while reading data, error message: CSV table references column position 49, but line contains only 2 columns.; line_number: 3 byte_offset_to_start_of_line: 42 column_index: 49 column_name: "trans_id" column_type: DOUBLE; reason: invalid, message: Error while reading data, error message: CSV table references column position 49, but line contains only 2 columns.; line_number: 4 byte_offset_to_start_of_line: 53 column_index: 49 column_name: "trans_id" column_type: DOUBLE; reason: invalid, message: Error while reading data, error message: CSV table references column position 49, but line contains only 2 columns.; line_number: 5 byte_offset_to_start_of_line: 61 column_index: 49 column_name: "trans_id" column_type: DOUBLE; reason: invalid, message: Error while reading data, error message: CSV table references column position 49, but line contains only 2 columns.; line_number: 6 byte_offset_to_start_of_line: 76 column_index: 49 column_name: "trans_id" column_type: DOUBLE; reason: invalid, message: Error while reading data, error message: CSV table references column position 49, but line contains only 2 columns.; line_number: 7 byte_offset_to_start_of_line: 86 column_index: 49 column_name: "trans_id" column_type: DOUBLE; reason: invalid, message: Error while reading data, error message: CSV table references column position 49, but line contains only 2 columns.; line_number: 8 byte_offset_to_start_of_line: 96 column_index: 49 column_name: "t

Looks a bit like a delimiter error, though I'm not sure. 

### Task 2

* Nice job on this task. 
* It'd be worth thinking through how you'd do this the reproducible way. That'd involve selecting distinct card numbers, pulling them down to python, calling `random.sample` and then building a query that could use them all. Let me know if it's not clear how you'd do that. 


### Task 3

You have errors in the queries on this task. Here's what you have for the second query: 

```
SELECT
    CAST(card_no AS INT64) AS card_no,  -- Owner ID
    EXTRACT(YEAR FROM datetime) AS year,
    EXTRACT(MONTH FROM datetime) AS month,
    SUM(total) AS total_sales,
    COUNT(datetime) AS num_transactions,
    SUM(CASE WHEN trans_status = ' ' OR trans_status = '' THEN 1 ELSE 0 END) AS num_items
FROM `{project_id}.{dataset_id}.transArchive_*`
WHERE CAST(card_no AS INT64) != 3  -- Exclude non-owners
GROUP BY card_no, year, month
ORDER BY card_no, year, month
```

Compare this to my solution, which is built on the work we did in Intro to SQL: 

```
SELECT card_no,
EXTRACT(YEAR from datetime) as year,
EXTRACT(MONTH from datetime) as month,
ROUND(SUM(total),2) AS sales,
COUNT(distinct(
  CONCAT(CAST(EXTRACT(DATE from datetime) AS STRING),
         CAST(register_no AS STRING),
         CAST(emp_no AS STRING),
         CAST(trans_no AS STRING)))) as transactions,
SUM(CASE WHEN (trans_status = 'V' or trans_status = 'R') THEN -1 ELSE 1 END) as items
FROM `umt-msba.wedge_transactions.transArchive_*`
 WHERE department != 0 and
      department != 15 and
     (trans_status IS NULL or 
      trans_status = ' ' or 
      trans_status = 'V' or 
      trans_status = 'R') 
GROUP BY card_no, year, month
ORDER BY card_no, year, month    
```

Otherwise this one looks great. 
