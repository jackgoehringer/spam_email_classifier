# Spam Email Classifier - Project Plan
## Group 9 - Jack Goehringer, Vincent Zhang
## Requirements List

1. **Data Loading**: Read the Spam or Not Spam Dataset from a CSV file.
2. **Email Feature Extraction**: Process each email to extract relevant features.
3. **Email Representation**: Create a data structure to represent emails with their features.
4. **Feature Storage**: Save email features and summary data to CSV files.
5. **Feature Display**: Display features of individual emails and summaries of groups.
6. **Distance Calculation**: Implement a method to calculate distance between two emails.
7. **Distance from Summary**: Calculate distance from an email to a summary of a group.
8. **Spam Classification**: Classify emails as spam or not spam based on distance metrics.
9. **Testing Framework**: Create tests to verify each requirement is met.

## Implementation Details

### 1. Data Loading
The program will use Java's built-in CSV parsing libraries to read the dataset. We'll create a DataLoader class that will:
- Open the CSV file
- Parse each row
- Create Email objects from the data
- Handle any file reading exceptions

```
Algorithm:
1. Open CSV file using BufferedReader
2. Read header line to identify columns
3. For each subsequent line:
   a. Split by comma
   b. Extract ID (column 1)
   c. Extract email text (column 2)
   d. Extract spam label (column 3)
   e. Create new Email object
   f. Add to collection
4. Close file
5. Return collection of Email objects
```

### 2. Email Feature Extraction
We'll create a FeatureExtractor class that will process each email and extract the following features:
- Word counts (frequency of each word)
- Total number of words
- Average word length
- Number of special characters
- Number of uppercase letters
- Number of numbers/digits

```
Algorithm:
1. Split email text into words
2. Initialize feature map
3. Count occurrences of each word
4. Calculate total word count
5. Calculate average word length
6. Count special characters
7. Count uppercase letters
8. Count numeric digits
9. Return feature map
```

### 3. Email Representation
We'll represent an email using an Email class that contains:
- Email ID
- Raw text
- Spam/not spam label (optional, may be unknown for test data)
- Map of extracted features

### 4. Feature Storage
The program will save features to CSV files using Java's file I/O capabilities:
- Individual email features will be saved to "email_features.csv"
- Summary data will be saved to "summary_features.csv"

### 5. Feature Display
We'll implement a simple console-based display for features:
- For individual emails: Display ID, label, and all features in a formatted table
- For group summaries: Display statistical information (min, max, mean, median, standard deviation)

### 6 & 7. Distance Calculation
We'll implement Euclidean distance as our metric:
- Between two emails: Calculate the square root of the sum of squared differences between each feature
- Between an email and a summary: Calculate distance using the mean values from the summary

### 8. Spam Classification
We'll use the summary-based approach for classification:
- Create summary models for spam and non-spam emails from training data
- For each test email, calculate distance to both models
- Classify based on the closest model

### 9. Testing Framework
We'll create JUnit tests for each requirement to verify functionality.

## UML Class Diagrams

```
+------------------------+       +------------------------+
|        Email           |       |    FeatureExtractor    |
+------------------------+       +------------------------+
| - id: int              |       | + extractFeatures(     |
| - text: String         |<------|   email: Email): Map   |
| - isSpam: Boolean      |       +------------------------+
| - features: Map        |              ^
+------------------------+              |
       ^                           +------------------------+
       |                           |  EmailSummaryGenerator |
+------------------------+         +------------------------+
|     EmailCollection    |-------->| + generateSummary(     |
+------------------------+         |   emails: List): Map   |
| - emails: List<Email>  |         +------------------------+
+------------------------+              ^
       ^                                |
       |                           +------------------------+
+------------------------+         |    DistanceCalculator  |
|      DataLoader        |         +------------------------+
+------------------------+         | + emailDistance(       |
| + loadData(            |         |   email1, email2): double |
|   filename: String)    |         | + summaryDistance(     |
+------------------------+         |   email, summary): double |
                                   +------------------------+
                                          ^
+------------------------+               |
|    CSVFileHandler      |         +------------------------+
+------------------------+         |   SpamClassifier       |
| + saveEmailFeatures(   |         +------------------------+
|   emails: List,        |         | + classify(            |
|   filename: String)    |         |   email: Email,        |
| + saveSummaryFeatures( |<--------|   spamModel: Map,      |
|   summary: Map,        |         |   nonSpamModel: Map)   |
|   filename: String)    |         +------------------------+
+------------------------+
```

## Questions and Answers

### 1. How do you plan to read the .csv file and process the individual cells of data?
I will use Java's BufferedReader and String.split() method to read and parse the CSV file. Each line will be split by commas, and the resulting values will be used to create Email objects. The DataLoader class will handle this functionality.

### 2. Which features do you plan to compute for each email?
For each email, I will compute:
- Word frequency counts (map of words to their occurrence count)
- Total word count
- Average word length
- Number of special characters
- Number of uppercase letters
- Number of numeric digits

### 3. How do you want to represent an email in code?
I will create an Email class that contains:
- An integer ID
- The raw text of the email
- A Boolean isSpam field for the spam/not spam label (which can be null if unknown)
- A Map<String, Double> for storing features and their values

This approach encapsulates all email information and allows for unknown spam labels in test data.

### 4. How do you plan to save the computed features and summaries?
I will create a CSVFileHandler class with methods to:
- Save email features to a CSV file with columns for email ID and each feature
- Save summary features to a separate CSV file with columns for feature name, mean, median, min, max, and standard deviation

### 5. How do you plan to display the computed features and the summaries?
I will implement console-based display methods that:
- For individual emails: Print a table with ID, label, and all features
- For summaries: Print statistical information for each feature

### 6. What distance metric do you plan to use for distance between two emails?
I will use Euclidean distance (L2 norm) between feature vectors. For each feature present in either email, I will:
1. Get the value from both emails (default to 0 if not present)
2. Calculate the squared difference
3. Sum all squared differences
4. Take the square root of the sum

### 7. How will you compute distance from an email to the summary features?
I will use the mean values from the summary as a representative "average email" and calculate the Euclidean distance between the email's features and these mean values.

## Testing Plan

1. **Data Loading Test**: Verify that emails are correctly loaded from CSV with proper IDs, text, and labels.
2. **Feature Extraction Test**: Verify that features are correctly extracted from sample emails.
3. **Email Representation Test**: Verify that Email objects properly store and retrieve all required data.
4. **Feature Storage Test**: Verify that features are correctly saved to CSV files.
5. **Feature Display Test**: Verify that features and summaries are displayed correctly.
6. **Distance Calculation Test**: Verify that distance calculations return expected values for known examples.
7. **Classification Test**: Verify that classification correctly identifies known spam and non-spam examples.

## Schedule

### Week 1
- Day 1-2: Implement Email class and DataLoader
- Day 3-4: Implement FeatureExtractor
- Day 5-7: Implement EmailSummaryGenerator

### Week 2
- Day 1-2: Implement CSVFileHandler for storage
- Day 3-4: Implement display functionality
- Day 5-7: Implement DistanceCalculator

### Week 3
- Day 1-2: Implement SpamClassifier
- Day 3-5: Create testing framework
- Day 6-7: Debug and finalize 