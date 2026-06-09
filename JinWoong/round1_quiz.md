df['age'] = fillna(df['age'].mean())
df = df.drop_duplicates(subset='customer_id')