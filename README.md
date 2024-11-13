# TLVArchiveAnalyzer
TLVArchiveAnalyzer
TLVArchiveAnalyzer is a Python-based data analysis tool designed to predict the true construction years of historical buildings based on a dataset of archived building documents. This script uses categorized document types, historical data, and a predictive linear model to estimate missing construction years.

Table of Contents
	•	Overview
	•	Data Input
	•	Installation
	•	Usage
	•	Project Structure
	•	Model and Prediction Process
	•	Category Weights per Period
	•	Output
	•	Contributing
	•	License

Overview
This script takes as input:
	1.	A table of building document details generated by the TLVArchive project.
	2.	A table of buildings with known construction years.
Using this data, it groups document types into categories and assigns weights to each category based on their frequencies per period for estimating true construction years. The script then outputs a CSV file with predicted years for all buildings.

Data Input
	•	True Years CSV: Contains records of known construction start years.
	•	Documents Data CSV: Contains records of document types and their associated dates, organized by tik_id (unique identifier for each building).

Installation
	1.	Clone the TLVArchiveAnalyzer repository: git clone https://github.com/yourusername/TLVArchiveAnalyzer.git
	2.	Download python > 3.8 and install required packages (pandas and pyarrow): pip install -r requirements.txt


Usage
To run the script, use:
python tlv_archive_analyzer.py --true_years PATH_TO_TRUE_YEARS_CSV --docs PATH_TO_DOCUMENTS_CSV [--export] [--print_scores]
	•	--export: Optionally export transformed data as CSV and Parquet files for faster loading in future runs. Second run will load the parquet file immediately without the need to read and parse big csv files.
	•	--print_scores: Export the intermediate scores calculated for each year-category pair.

Example

python tlv_archive_analyzer.py --true_years years.csv --docs all_docs.csv --export --print_scores

Project Structure
•	Reading and parsing: Reads the csv files and parse them into 2 tables.
•	Pivoting and Merging: Merges the tables based on tik_id.
•	Category Assignments: Groups document types into predefined 3 categories.
•	Weight Calculation: Assigns weights to categories based on document frequency within 4 specific historical periods.
•	Prediction: Predicts missing construction years based on calculated weights.

Model and Prediction Process
	1.	Define Document Categories: Types of building-related documents are grouped into three categories for easier processing:
	•	Category 1: [“היתר-תכנית חתומה”, “היתר מילולי חתום”]
	•	Category 2: [“הודעת שומה על היטל השבחה,אגרות בניה,שוברי תשלום”]
	•	Category 3: [“תעודת גמר”, “תיק תעודת גמר”, “טופס 4”, “טופס 1”]
	2.	Define Time Periods: Construction year predictions are segmented by four historical periods:
	•	1920-1925
	•	1926-1943
	•	1944-1985
	•	1986-2021
	3.	Calculate Weights: For each period and category, a weight is calculated based on document frequency within a 3-year gap from the true construction year.

1920-1925 - not relevent, if catagory 1 exist, the year is marked.

Category Weights per Period
Period
category1
category2
category3
1926-1943
0.510753
0.238028
0.163686
1944-1985
0.321881
0.283602
0.53169
1986-2021
0.332795
0.424807
0.559186
 
Note: These weights are calculated dynamically based on the input data.

Output
	predicted_years.csv: Contains predictions for construction years, along with prediction scores for each document category.

Contributing

Contributions are welcome! Please fork this repository and submit pull requests.

License

This project is licensed under the MIT License.
