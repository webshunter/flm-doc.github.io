# MySQL Functions

## 1. nama_majikan Function

### Function Definition
```sql
DELIMITER $$

CREATE FUNCTION nama_majikan(p_id_biodata VARCHAR(50))
RETURNS VARCHAR(255)
DETERMINISTIC
BEGIN
    DECLARE v_namamajikan VARCHAR(255);

    SELECT IFNULL(NULLIF(NULLIF(m.namamajikan, ''), '0'), d.nama)
    INTO v_namamajikan
    FROM majikan m
    LEFT JOIN datamajikan d ON m.kode_majikan = d.id_majikan
    WHERE m.id_biodata = p_id_biodata
    LIMIT 1;

    RETURN v_namamajikan;
END$$

DELIMITER ;
```

### Function Description
- **Purpose**: Retrieves the employer's name (nama majikan) based on biodata ID with fallback logic
- **Parameter**: 
  - `p_id_biodata` (VARCHAR(50)): The biodata ID to look up
- **Return**: VARCHAR(255) - The employer's name

### Logic Flow
1. First checks `m.namamajikan` from majikan table
2. If empty or '0', falls back to `d.nama` from datamajikan table
3. Uses LEFT JOIN to ensure all records are returned even without matching datamajikan

### Usage Example
```sql
SELECT nama_majikan('BIODATA123'); -- Returns employer name for biodata ID 'BIODATA123'
```

### Important Notes
- Function is DETERMINISTIC (always returns same result for same input)
- Uses IFNULL and NULLIF for fallback logic
- Joins majikan and datamajikan tables
- Returns NULL if no matching record found
- Used in various queries to get employer information
- Handles empty strings and '0' values as NULL

### Database Tables Used
- `majikan` - Contains employer information linked to biodata
- `datamajikan` - Contains master data for employers

### Implementation Details
- Uses LEFT JOIN to ensure all biodata records are returned
- Implements fallback logic for missing or invalid employer names
- Optimized with LIMIT 1 for performance
- Handles edge cases like empty strings and '0' values 