# download enron email databases
!wget -O enron.db https://curtin-my.sharepoint.com/:u:/g/personal/211934g_curtin_edu_au/EaYagsqa2r1Bi5wtHbswGFwBH2kd2uTnz6rlka7GI36GUQ?download=1
import sqlite3

conn = sqlite3.connect("enron.db")
cur = conn.cursor()
# Retrieving table names
sql = """
SELECT name
FROM sqlite_master
WHERE type = "table";
"""
cur.execute(sql)
tables = cur.fetchall()

# Retrieving table information
for table in tables:
    table_name = table[0]
    sql = f"PRAGMA table_info('{table_name}');"
    cur.execute(sql)
    table_info = cur.fetchall()
    print(f"Table: {table_name}")
    for column in table_info:
        print(column)
    print("\n")

import pandas as pd

# Creating employeelist dataframe
sql = "SELECT * FROM employeelist;"
employeelist_df = pd.read_sql_query(sql, conn)

# Creating message dataframe
sql = "SELECT * FROM message;"
message_df = pd.read_sql_query(sql, conn)

# Creating recipientinfo dataframe
sql = "SELECT * FROM recipientinfo;"
recipientinfo_df = pd.read_sql_query(sql, conn)

