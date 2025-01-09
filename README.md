# Flow Logs Analyzer

A Python tool to analyze network flow logs and map them to tags based on destination port and protocol combinations.

## Features

- Parse and analyze AWS VPC Flow Logs
- Map log entries to tags using a lookup table
- Case-insensitive protocol matching
- Generate statistics for tag counts and port/protocol combinations
- Handle large files efficiently

## Requirements

- Python 3.6+
- Pandas library (since we're using Jupyter Notebook)

You can install pandas using pip:
```bash
pip install pandas
```

Or if you have a requirements.txt file:
```bash
pip install -r requirements.txt
```
To run a jupyter notebook just click on the run button
or open the notebook and shift + enter a particular cell

## File Structure

```
.
├── Assessment.ipynb     # Jupyter notebook containing the analysis code
├── README.md           # This documentation file
├── requirements.txt    # To install Pandas library
├── flow_analyzer.py    # Main program file (if you want to run outside notebook)
├── lookup_table.csv    # CSV file containing port/protocol to tag mappings
└── log_data.csv        # Input flow logs file
```

## Assumptions

1. Flow Log Format:
   - Only supports AWS VPC Flow Logs version 2
   - Custom log formats are not supported
   - Fields must be space-separated in the default order
   - Jupyter notebook is installed

2. Input Files:
   - All files must be ASCII text files
   - Maximum flow log file size: 10MB
   - Maximum lookup table mappings: 10000
   - Files must be in the same directory as the script

3. Protocol Handling:
   - Supports TCP (6), UDP (17), and ICMP (1)
   - Other protocols are marked as 'unknown'
   - Protocol matching is case-insensitive

4. Error Handling:
   - Invalid log entries are skipped with warnings
   - Empty lines are ignored
   - Missing input files trigger appropriate error messages

## Input File Formats

### Flow Logs (log_data.csv)
```
version account-id interface-id srcaddr dstaddr srcport dstport protocol packets bytes start end action log-status
2 123456789012 eni-0a1b2c3d 10.0.1.201 198.51.100.2 443 49153 6 25 20000 1620140761 1620140821 ACCEPT OK
```

### Lookup Table (lookup_table.csv)
```csv
dstport,protocol,tag
25,tcp,sv_P1
443,tcp,sv_P2
110,tcp,email
```

## Running the Program

### Using Jupyter Notebook
1. Ensure pandas is installed (see Requirements section)
2. Open `Assessment.ipynb` in Jupyter Notebook or JupyterLab
3. Run all cells to execute the analysis

Note: While the core analysis code uses only standard library functions, pandas is required for running the Jupyter notebook version of this 

### Using Python Script
1. Ensure your input files are in the same directory
2. Run:
```bash
python flow_analyzer.py
```

The program will create `analysis_report.csv` with the results.

## Testing Performed

1. Input Validation Tests:
   - Invalid flow log versions
   - Malformed log entries
   - Empty lines
   - Invalid port numbers
   - Unknown protocols

2. Lookup Table Tests:
   - Case sensitivity
   - Missing columns
   - Invalid port numbers
   - Whitespace handling

3. Performance Tests:
   - Large file handling (up to 10MB)
   - Multiple port/protocol combinations
   - Various tag mappings

4. Error Handling Tests:
   - Missing input files
   - Invalid file formats
   - Invalid data types

## Code Analysis

1. Memory Efficiency:
   - Uses file streaming to process logs line by line
   - Minimizes memory usage for large files
   - Uses efficient data structures (dictionaries, defaultdict)

2. Performance:
   - O(n) time complexity for log processing
   - O(1) lookup time for tag matching
   - Efficient string operations

3. Maintainability:
   - Well-documented code with type hints
   - Clear error messages
   - Modular design with separate concerns

4. Extensibility:
   - Easy to add new protocol types
   - Can be modified to support other log formats
   - Configurable output format

## Contributing

Feel free to submit issues and enhancement requests.

## License

This project is licensed under the MIT License.