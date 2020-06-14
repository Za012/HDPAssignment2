# Grade 6
--------------------------------------------------------------------------------
```
ordersCSV = LOAD '/user/maria_dev/Diplomacy/orders.csv'  USING PigStorage(',')
AS (game_id:chararray, unit_id:chararray, unit_order:chararray, location:chararray, target:chararray, target_dest:chararray, success:chararray, reason:chararray, turn_num:chararray);

-- To get the list in alphabetic order we use the ORDER command together with ASC, standing for ASCENDING
order_by_alphabet = ORDER ordersCSV BY location ASC; 

-- I tried to FILTER using target == 'Holland', but that didn't work out, instead I resorted to using a Regular Expression with the MATCHES command
ordersCSV = FILTER order_by_alphabet BY (target MATCHES '.*Holland.*');

-- Now our dataset is clean and the answer is ready to be extracted >????
A = FOREACH (GROUP ordersCSV BY location) GENERATE FLATTEN (group), FLATTEN (ordersCSV.target), COUNT($1);
B = DISTINCT A;
C = ORDER B BY $0 ASC;

DUMP C;
```
# Grade 7
---------------------------------------------------------------------------------
```
playersCSV = LOAD '/user/maria_dev/Diplomacy/players.csv'  USING PigStorage(',')
AS (game_id:chararray, country:chararray, won:chararray, num_suppy_centers:chararray, eliminated:chararray, start_turn:chararray, end_turn:chararray);

-- Removing the matches that were a loss
X = FILTER playersCSV BY (won matches '.*1.*');

-- Iterate 
A = FOREACH (GROUP X BY country) GENERATE FLATTEN(group), COUNT($1);

DUMP A;
```
