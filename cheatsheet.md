# connect
db2 connect to ''

# MERGE
MERGE INTO table_to_upsert AS tab
USING (VALUES
        (1, 2, 3),
        (4, 5, 6),
        (7, 8, 9)
        -- more rows
    ) AS merge (C1, C2, C3)
    ON tab.key_to_match = merge.key_to_match
    WHEN MATCHED AND tab.C1 != merge.C1 THEN
        UPDATE SET tab.C1 = merge.C1,
                   tab.C2 = merge.C2,
                   tab.C3 = merge.C3
    WHEN NOT MATCHED THEN
        INSERT (C1, C2, C3)
        VALUES (merge.C1, merge.C2, merge.C3)