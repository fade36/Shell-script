# Automated 24-Hour Directory Backup Script

A lightweight and robust Bash script designed to automate incremental data backups. It scans a source directory for any files or folders modified within the last 24 hours, bundles them into a timestamped, compressed archive (`.tar.gz`), and safely moves the archive to a designated destination folder.

This project was developed as part of the **IBM Linux Commands & Shell Scripting** certification.

## 🚀 Features
* **Automated Incremental Backup:** Dynamically isolates only the files modified within the past 24 hours.
* **Epoch Timestamping:** Uses Unix Epoch timestamps (`date +%s`) to guarantee unique archive filenames, preventing data overwrite.
* **Robust Path Management:** Automatically resolves relative input paths into robust absolute system paths (`pwd`) before execution.
* **Input Validation:** Prevents syntax crashes by strictly enforcing arguments and validating directory existence before initiating the archive.
* **Cron-Ready:** Packaged and configured to be safely deployed as a background daemon system job.

## 🛠️ Architecture & Logic
1. **Validation:** Checks if exactly two arguments are provided (`$1` as source, `$2` as destination) and verifies both paths actually exist (`[ -d ]`).
2. **Time Window Calculation:** Fetches the current timestamp in seconds and calculates `yesterdayTS` by subtracting 86,400 seconds (24 hours).
3. **Target Filtering:** Iterates through the working directory using a wildcard loop `for file in *`.
4. **Data Aggregation:** Uses POSIX safe variables and Bash Arrays (`declare -a toBackup`) to bundle target paths, preventing bugs caused by files with empty spaces.
5. **Archiving:** Executes a single `tar -czvf` instruction on the array, compressing files into a `.tar.gz` archive, then executes an internal `mv` to the absolute destination path.

