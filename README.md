# Employee Performance Review Data Processing Automation

This PowerShell script automates the processing and analysis of employee performance review data. It reads data from a CSV file, calculates performance scores, categorizes employees based on their scores, and generates a summary report.

## Objective

To create a PowerShell script that:
- Reads employee data from a CSV file.
- Calculates average performance scores for each employee.
- Categorizes employees based on their average scores.
- Generates a summary report.

### Input Data

The script reads data from a CSV file named `EmployeePerformance.csv`. The CSV file contains the following columns:
- `EmployeeID`
- `Name`
- `Department`
- `ReviewScore1`
- `ReviewScore2`
- `ReviewScore3`

### Calculations

- Calculate the average performance score for each employee.
- Categorize employees based on their average score:
  - **Outstanding**: Average score >= 90
  - **Exceeds Expectations**: Average score >= 75 and < 90
  - **Meets Expectations**: Average score >= 50 and < 75
  - **Needs Improvement**: Average score < 50

### Output

- Generate a summary report listing each employee, their department, average score, and category.
- Output the summary to a new CSV file named `PerformanceSummary.csv`.
- Print a message indicating the completion of the process.

## Script Structure

1. **Reading Data**: Use `Import-Csv` to read the input CSV file.
2. **Processing Data**:
   - Use loops to iterate through each employee's data.
   - Use control flow statements to categorize employees based on their average scores.
3. **Generating Output**:
   - Use `Export-Csv` to save the summary report to a new CSV file.
4. **Code Reusability**: Create reusable functions for calculating average scores and categorizing employees.

## PowerShell Script

```powershell
# Function to calculate the average score
function Get-AverageScore {
    param (
        [float]$score1,
        [float]$score2,
        [float]$score3
    )
    return ($score1 + $score2 + $score3) / 3
}

# Function to categorize the employee based on the average score
function Get-Category {
    param (
        [float]$avaragescore
    )
    if ($avaragescore -ge 90) {
        return "Outstanding"
    } elseif ($avaragescore -ge 75 -and $avaragescore -lt 90) {
        return "Exceeds Expectations"
    } elseif ($avaragescore -ge 50 -and $avaragescore -lt 75) {
        return "Meets Expectations"
    } else {
        return "Needs Improvement"
    }
}

# Read employee data from CSV
$employees = Import-Csv -Path "D:\College\Sam_4\SYST16023 MS Powershell Scripting\Week9\EmployeePerformance.csv"

# Create an array to hold the processed data
$processedData = @()

# Process each employee's data
foreach ($employee in $employees) {
    $avaragescore = Get-AverageScore -score1 $employee.ReviewScore1 -score2 $employee.ReviewScore2 -score3 $employee.ReviewScore3
    $category = Get-Category -avaragescore $avaragescore

    $processedEmployee = [PSCustomObject]@{
        EmployeeID = $employee.EmployeeID
        Name = $employee.Name
        Department = $employee.Department
        AverageScore = [math]::Round($avaragescore, 2)
        Category = $category 
    }
    $processedData += $processedEmployee
}

# Export the processed data to a new CSV file
$processedData | Export-Csv -Path "PerformanceSummary.csv" -NoTypeInformation

# Print completion message
Write-Output "Employee performance review processing completed. Summary saved to PerformanceSummary.csv."
```

## Usage

1. Place the input CSV file (`EmployeePerformance.csv`) in the appropriate directory.
2. Run the PowerShell script.
3. Check the output CSV file (`PerformanceSummary.csv`) for the summary report.

## License

This project is licensed under the MIT License.

## Acknowledgements

This script was created as part of an assignment to demonstrate automation using PowerShell.
