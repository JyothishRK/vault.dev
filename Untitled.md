### Example commands with ranges

All commands run from `src/` with env loaded (e.g. `.env.development` or `.env.uat`).

- **Full file (default behavior)**  
  ```bash
  sails run update-user-emails
  ```

- **Dry run on full file (no DB writes)**  
  ```bash
  sails run update-user-emails --dryRun=true
  # or
  DRY_RUN=1 sails run update-user-emails
  ```

- **Use a custom Excel file**  
  ```bash
  sails run update-user-emails --excelPath=scripts/data/kfshrc/user_email_mapping.xlsx
  # or
  EXCEL_PATH=/path/to/file.xlsx sails run update-user-emails
  ```

- **First 500 data rows only**  
  ```bash
  sails run update-user-emails --endRow=500
  ```

- **Rows 501 to 1000 (inclusive)**  
  ```bash
  sails run update-user-emails --startRow=501 --endRow=1000
  ```

- **Row 1000 to end of file**  
  ```bash
  sails run update-user-emails --startRow=1000
  ```

- **Dry run on a specific range**  
  ```bash
  sails run update-user-emails --dryRun=true --startRow=1 --endRow=100
  ```