# OpenMP Parallel Program for Data Processing and Sum Calculation

## **Overview**

This program extends the functionality of **Program A** by utilizing **OpenMP** to handle data processing across multiple threads. The program reads data from a file, divides the work among threads manually, and computes the sums of specific fields (e.g., `int` and `float`) using OpenMP synchronization tools. The main goal is to compute filtered results and append the sums of certain fields to the result file.

The threads do not rely on monitors for synchronization but instead manually distribute data as evenly as possible, ensuring that the number of elements each thread processes is as balanced as possible. The program computes sums only for the elements that meet a specific filter criterion. The results are written to a result file, which includes both the processed data and computed sums.

## **Key Features**

- **Data Distribution**: Manually divides data among threads without using OpenMP parallel loops. The number of elements each thread processes is as balanced as possible.
  
- **OpenMP Synchronization**: Utilizes OpenMP tools for managing critical sections when computing sums of `int` and `float` fields, ensuring thread-safe summation.

- **Parallel Processing**: Threads process data concurrently, each working on a portion of the data and computing the results according to a filter criterion.

- **File I/O**: The program reads input data from a JSON file, processes it, and outputs the sorted results along with computed sums to a result file.

- **Result Format**: In addition to the processed data, the result file includes computed sums for the `int` and `float` fields that meet the filter criteria.

## **Input & Output Files**

- **Input File**: JSON formatted file containing student data (e.g., name, year, grade).
  
  Example format:
  ```json
  {
    "students": [
      {"name": "Antanas", "year": 1, "grade": 6.95},
      {"name": "Kazys", "year": 2, "grade": 8.65},
      {"name": "Petras", "year": 2, "grade": 7.01},
      // More student data...
    ]
  }
  ```

- **Output File**: A text file containing:
  - The processed student data sorted by hash.
  - The sums of `int` and `float` fields computed for the filtered data.

  Example output format:
  ```txt
  Name | Year | Grade | Hash
  ----------------------------------------
  Jonas | 1 | 6.95 | A18EAC8F30AC0FC630AE175A851CA5DA24FA8C85
  Kazys | 2 | 8.65 | E763185F4B7303A787ACC513B7AA56706C7A42AC
  // More processed data...
  Total int sum: 10
  Total float sum: 200.75
  ```

## **How it Works**

1. **Data Parsing**: The program reads the JSON input file containing student data, which includes fields like `name`, `year`, and `grade`.

2. **Data Distribution**: The data is divided as evenly as possible across multiple threads (2 ≤ number of threads ≤ n/4, where n is the number of data items). If there are 27 data items and 6 threads, some threads will process 4 items, while others will process 5 items.

3. **Parallel Processing**: In each thread:
   - The data is filtered using a custom criterion.
   - The `name`, `year`, and `grade` fields are concatenated and hashed using the SHA1 algorithm.
   - Only data with hashes starting with a letter (not a digit) are kept.
   - The hash-sorted results are stored in a shared structure, and the sums of the `int` and `float` fields are computed using OpenMP tools.

4. **Output**: After all threads complete processing, the results are written to a result file, which contains the filtered, sorted data, and the computed sums.

## **Technologies Used**

- **OpenMP** for parallel processing and thread synchronization.
- **C++** programming language with OpenMP support enabled.
- **JSON** format for input and output data files.
- **SHA1** hash function for filtering the data.
  
## **Requirements**

- **C++ Compiler** with OpenMP support:
  - **Windows (Visual Studio)**: Enable OpenMP support in project settings.
  - **Ubuntu (g++)**: Use `-fopenmp` flag when compiling.
  - **CMake**: Add the `-fopenmp` flag to `CMakeLists.txt`.
  
- **Libraries**:
  - **nlohmann JSON** library for parsing and writing JSON data.

## **Usage**

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/OpenMP-Data-Processing.git
   ```

2. Compile the program with OpenMP support enabled:
   ```bash
   g++ -o program -fopenmp program.cpp
   ```

3. Run the program:
   ```bash
   ./program
   ```

4. The program will read data from the input file, process it with OpenMP, and write the sorted results and computed sums to the output file.

## **License**

This project is licensed under the MIT License
