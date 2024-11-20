
# PostgreSQL JSON Querying Cheat Sheet

## 1. **Check Column Type**:
Ensure your column is of type `json` or `jsonb`. You can check it by running:

```sql
\d your_table_name;
```

## 2. **Extract a Specific Key as Text**:
Use the `->>` operator to **extract a key's value as text** from a JSON object.

### Example:
Extract the value of `form_count` as text:

```sql
SELECT json_data->>'form_count' AS count_value
FROM your_table_name;
```

### Explanation:
- `json_data->>'form_count'`: Extracts the value of `form_count` as text.
- `AS count_value`: Aliases the extracted value as `count_value`.

## 3. **Extract Nested JSON Keys**:
If the key is inside a nested object, use the `->` operator first, followed by `->>`.

### Example:
If `form_count` is nested inside the `page` object:

```sql
SELECT json_data->'page'->>'form_count' AS count_value
FROM your_table_name;
```

## 4. **Cast to a Specific Data Type**:
If the value needs to be cast to another type (e.g., integer), use `::` to cast it.

### Example:
Cast the `form_count` value to an integer:

```sql
SELECT (json_data->>'form_count')::int AS count_value
FROM your_table_name;
```

## 5. **Common Mistakes to Avoid**:
- **Using `->` instead of `->>`**:  
  - `->` returns the JSON object, not a text value.  
  - `->>` returns the value as text.
- **Incorrect Nested Key Access**:  
  - Ensure you use `->` for JSON objects and `->>` for text extraction.

## Quick Reference:
- **`->`**: Extracts a JSON object.
- **`->>`**: Extracts the value as text.
- **Casting to Integer**: Use `::int` to cast the extracted value to an integer.
- **Nested Keys**: Chain `->` and `->>` for nested objects.

## Examples:

### 1. **Simple Query** (Extract a single key value):
```sql
SELECT json_data->>'form_count' AS count_value
FROM your_table_name;
```

### 2. **Nested JSON Key**:
```sql
SELECT json_data->'page'->>'form_count' AS count_value
FROM your_table_name;
```

### 3. **Casting to Integer**:
```sql
SELECT (json_data->>'form_count')::int AS count_value
FROM your_table_name;
```

### 4. **Count Rows Based on a Key**:
```sql
SELECT COUNT(*)
FROM your_table_name
WHERE (json_data->>'form_count')::int > 0;
```

---

## Final Tip:
- **Remember**:
  - **`->>`** = Extracts value as **text**.
  - **`->`** = Extracts **JSON object**.
