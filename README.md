# Audacity Labels Time Format Converter

This Python script converts the times in an Audacity labels file from seconds to `hh:mm:ss.sss` format.

## Prerequisites

- Python 3.x installed on your system.

## Getting Started

Follow these instructions to use the script for converting your Audacity labels file.

### Clone the Repository

First, clone the repository to your local machine.

```bash
git clone https://github.com/yourusername/audacity-labels-converter.git
cd audacity-labels-converter
```

### Prepare Your Labels File

Ensure you have your Audacity labels file (e.g., `labels.txt`) in the same directory as the script. The format of this file should be:

```
start_time   end_time   label_text
```

Example:
```
5500.946576	6284.740020	true_or_false_round
5502.862222	5538.597732	true_or_false_instruction
```

### Running the Script

Run the script to convert the times in your labels file from seconds to `hh:mm:ss.sss` format.

```bash
python convert_labels.py
```

### Output

The script will create a new file called `labels_converted.txt` in the same directory with the converted times.

Example output in `labels_converted.txt`:
```
01:31:40.947	01:44:44.740	true_or_false_round
01:31:42.862	01:32:18.598	true_or_false_instruction
```

## Script Explanation

The script consists of two main functions:

### `seconds_to_hms(seconds)`

This function converts time in seconds to `hh:mm:ss.sss` format.

```python
def seconds_to_hms(seconds):
    """
    Convert seconds to hh:mm:ss.sss format.

    Args:
        seconds (float): Time in seconds.

    Returns:
        str: Time in hh:mm:ss.sss format.
    """
    h = int(seconds // 3600)  # Calculate hours
    m = int((seconds % 3600) // 60)  # Calculate minutes
    s = seconds % 60  # Calculate remaining seconds
    return f"{h:02}:{m:02}:{s:06.3f}"  # Format as hh:mm:ss.sss
```

### `convert_labels_file(input_file, output_file)`

This function reads the input labels file, converts the times, and writes them to the output file.

```python
def convert_labels_file(input_file, output_file):
    """
    Convert times in an Audacity labels file from seconds to hh:mm:ss.sss format.

    Args:
        input_file (str): The path to the input file containing labels.
        output_file (str): The path to the output file to save converted labels.
    """
    # Open the input file for reading and the output file for writing
    with open(input_file, 'r') as infile, open(output_file, 'w') as outfile:
        # Process each line in the input file
        for line in infile:
            # Split the line into parts (start time, end time, label text)
            parts = line.strip().split()
            # Check if the line has exactly 3 parts
            if len(parts) == 3:
                start_time, end_time, label = parts
                # Convert start and end times from seconds to hh:mm:ss.sss format
                start_time_hms = seconds_to_hms(float(start_time))
                end_time_hms = seconds_to_hms(float(end_time))
                # Write the converted times and label to the output file
                outfile.write(f"{start_time_hms}\t{end_time_hms}\t{label}\n")
```

## Example Usage in Script

The script is executed by specifying the input and output file names.

```python
# Example usage:
input_file = 'labels.txt'  # Name of the input file with labels
output_file = 'labels_converted.txt'  # Name of the output file for converted labels
convert_labels_file(input_file, output_file)  # Convert and save the labels
```
