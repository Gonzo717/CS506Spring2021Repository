# Project WGBH DCAMM
## Problem Statement
All contractors commissioned by the state for major construction projects need to report their ethnic and gender makeup of the work forces. The WGBH would like to understand the data contained in those Summary of Workforce Utilization reports. Furthermore, the WGBH is interested in getting data-driven insights of the impact drawn upon specific groups of working forces between 2019 to 2020. The data is given in PDF format and organized by hours spent per project per organization.  Our goal is to first extract data in proper formats from the PDF files and then run some analysis.

## Weekly Meeting with the PM, Lingyan Jiang , is Thurs 11:30 AM - 1:00 PM
### With WGBH, Paul Singer, - every other Thurs 11:30 AM - 1:00 PM  
Second meeting with the client on Thurs March 4th  
Spark Liason - Greta Bruce

## Contact List

Client Paul Singer <paul_singer@wgbh.org>,  
Spark Liason Greta Bruce <gretab@bu.edu>,   
PM Lingyan Jiang <lingyanj@bu.edu>,  
Students Rep Jena Jordahl <jenajj@bu.edu>,  
Elisa Cordeiro Lopes <elisacl@bu.edu>, Richard Lee <rlee99@bu.edu>
Murtadha - Ahmad M Al Bahrani<murtadha@bu.edu>
Carmen - Sabrina Araujo<sabrinaa@bu.edu>

Github accounts  
elisa3lopes, rlee99, murtio, carmen-araujo, jenajjedu

## Data Sources
The data is collected weekly by DCAMM. They sort it by months and keep it in PDF form. DCAMM already provided WGBH the work force from 2019 and will provide in March the data from 2020. The data is organized as tables of projects (such as bridges, buildings, etc) containing the companies included, their types of workers, and the hour rate separated by race, sex, and ethnicy. For this project, no additional datasets are required to be extracted, but our team is open to get any other information as it seems relevant to analysis. 
An example of a file is April 2019: https://drive.google.com/file/d/1brxGTjfkhwKRXPAbzDwHl4bP6J08Xwtz/view?usp=sharing  
We have been given a file folder with files for each month Jan - Dec 2019, e.g. WorkforceUtilizationSummaryReportApril2018.pdf



## Methods
We use two python libraries and an interactive tabula desktop program to parse the workforce utilization pdf files.

To help us configure the tabula library variables correctly, 
 we use the desktop tabula program to save the bounding box template x1, y1, x2, y2 variables. Following the instructions provided on 
tabula's website, the user drags a mouse over the document to be parsed to define the x, y coordinates.
Then the user saves this information as a json template file.  By reading the values out of the file, we configured 
the tabula python library to read the data frames on each page. The configuration used to parse 2019 and 2020 files is given below.  

df0 = tabula.read_pdf(filename, pages=proc_pages, lattice=True, area=(11, 26, 582, 829),
                              pandas_options={'header': None})

The pandas option to not use headers keeps data from being misinterpreted as column headings.                              
                              
The python library PyPDF2 reads number of pages in the pdf data file. By knowing the number of pages
 we set up a loop to read each page and every dataframe to build an analysis dataframe. The system reads 
 one month's worth of data and produces a dataframe that it writes to a .csv file  for each month.
 
 The pdf reports were set up with white space to assist the human reader in noticing when a group of data is being subtotaled or totaled. 
 For analysis, all these blank frames have to filled with the appropriate data.  
 
Once the monthly .csv files are finished, we use jupyter notebooks to read the csv files into a single yearly dataframe.  In the main directory, the "other.csv" file is example output from combining the monthly files.

The by_trade.csv file in the main directory gives an example of saved work compiling statistics used to create the graphics on our report.


## Commands 
Step 1- Create Environment
Create your virtual environment as you like it. 
The bash script "startenv.sh" can be run at the command line to create your virtual environment. You can also run the commands bellow in your terminal. Make sure the requirements.txt is on the correct directory.
```
cd to working directory
conda create -n env_name
conda activate env_name
pip install -r requirements.txt
```

Step 2- Run parser
Before running the parser commands be sure to use a 3 letter abreviation for the month in the pdf file name. Capitalize the first letter of the abbreviation. Also, make sure all the pdfs are in the same directory as the color-of-money files.

Run the command python3 ```color-of-money.py``` to parse the 2019 data.

Run the command ```python3 color-of-money20.py``` to parse the 2020 data. 

Each command produces a proof file: proof_2019.txt and proof_2020.txt respectively. They will also provide the correctly formatted CSV file, named like:  WorkforceUtilizationSummary2020.csv.

Step 3- Later years
add here the change sto do in color-of-money to parse 21 data.
There is no guarantee it will work for 2021 data. If the formatting is the same as 2019 and 2020, our parser should work.
 

## Discussion and Limitations
Note: The client wanted the data extracted from PDF files and said we would discuss specific questions for analysis after this was completed. Fortunately, we were able to convert PDF files into pandas dataframes for analysis and graphing. Then, we found patterns or trends to answers questions revolving around race, sex, and opportunities for state contracts. Specific questions that will be answered: 
How will we extract data from our PDF files?
Is there a difference between state-paid contractual hours based on color and/or sex?
What are the factors, e.g. location of the project, that fair in hiring working crews?
How state-wise elections affect hiring decisions across projects?

## PROOF
The way the system works. Because we found ounly 2% participation for some ethnicities, we wanted to write a parse that could prove that it consumed every row of the file correctly. Due to time limitations, we could not check every line,
therefore, what we did was randomly select a few hundred data frames and compare the numbers and quantities of rows consumbed. 

Below is an example.  Note the pattern, every data frame lists its number of rows and columns: [22 rows x 12 columns]
This number can easily be compared with the last section of the parser print lines.  By adding together the row number processed and the number of lines in the group, the tester can validate that the number of rows were all parsed. 
For instance, row number 17 + trade 5 = 22 and the data frame rows states there were 22 rows.

"Processing row number: 17
 trade 5"

The full data rows were verified separately and no misallignment was found when running group by commands on the pandas dataframes. Again, this was not tested exhaustively and instead a random set of a hundred rows were sampled and verified.


The proof.txt file allows one to verify individual rows on the pdf with the page parsed on atleast two values per row.

--------------- excerpt from proof2020.txt
 Processing row number: 0
['MONTH', 'YEAR', 'PROJECT', 'PROJECT_CODE', 'CONTRACTOR', 'CONSTRUCTION_TRADE', 'CRAFT_LEVEL', 'TOTAL_EMPLOYEE', 'CAUCASIAN', 'AFRICAN_AMERICAN', 'HISPANIC', 'ASIAN', 'NATIVE_AMERICAN', 'OTHER', 'NOT_SPECIFIED', 'TOTAL_FEMALE', 'TOTAL_MALE', 'HOURS_WORKED_PER_MONTH']
 Processing DF number: 1
                                     0           1   ...     10        11
0   Arden Engineering Constructors, LLC         NaN  ...    NaN       NaN
1                            PIPEFITTER     Journey  ...    0.0      0.00
2                                   NaN  Apprentice  ...    0.0    117.00
3                                   NaN   A/J Ratio  ...    0.0      0.00
4                                   NaN    New Hire  ...    0.0      0.00
5                                   NaN    Subtotal  ...    0.0    117.00
6              PIPEFITTER & STEAMFITTER     Journey  ...  168.0    989.00
7                                   NaN  Apprentice  ...    0.0      0.00
8                                   NaN   A/J Ratio  ...    0.0      0.00
9                                   NaN    New Hire  ...    0.0      0.00
10                                  NaN    Subtotal  ...  168.0    989.00
11                 Total for Contractor     Journey  ...  168.0    989.00
12                                  NaN  Apprentice  ...    0.0    117.00
13                                  NaN   A/J Ratio  ...    0.0      0.12
14                                  NaN    New Hire  ...    0.0      0.00
15                                  NaN    Subtotal  ...  168.0  1,106.00
16          Axion Specialty Contracting         NaN  ...    NaN       NaN
17   ASBESTOS INSULATOR (PIPES & TANKS)     Journey  ...    0.0      0.00
18                           Apprentice        2.00  ...    2.0       NaN
19                            A/J Ratio        0.00  ...    0.0       NaN
20                             New Hire        0.00  ...    0.0       NaN
21                             Subtotal        2.00  ...    2.0       NaN

[22 rows x 12 columns]

 Processing row number: 0
 complete grp cj  Arden engineering constructors, llc  1 
 complete grp contractor

 Processing row number: 1
 trade 5
 process grp j
 process grp a
 process grp aj
 process grp nh
 process grp sub
 complete grp trade

 Processing row number: 6
 trade 5
 process grp j
 process grp a
 process grp aj
 process grp nh
 process grp sub
 complete grp trade

 Processing row number: 11
 contr_total 5
 process grp j
 process grp a
 process grp aj
 process grp nh
 process grp sub
 complete grp contr_total

 Processing row number: 16
 complete grp cj  Axion specialty contracting  1 
 complete grp contractor

 Processing row number: 17
 trade 5
 process grp j
 process grp a
 process grp aj
 process grp nh
 process grp sub
 complete grp trade
 
 --------------------
