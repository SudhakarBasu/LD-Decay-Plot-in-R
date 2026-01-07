# LD-Decay-in-R
To extract linkage disequilibrium (LD) information from Tassel software, you typically need to perform a series of steps using TASSEL's LD analysis tools. LD analysis in TASSEL allows you to examine the extent of non-random association of alleles at different loci in a population. Here's a general guide on how to do this:

**1.	Data Preparation:**

•	Ensure your genotype data is in a compatible format for TASSEL. Common formats include HapMap or VCF.

•	Make sure your data is cleaned and filtered for quality and missing data if necessary.

**2.	Open TASSEL:**

•	Launch TASSEL software on your computer.

**3.	Import Data**:

•	Import your genotype data into TASSEL.

•	Navigate to the "Data" tab and use the import tools to load your genotype data file.

**4.	Perform LD Analysis:**

•	Once your data is loaded, navigate to the "LD" tab or menu in TASSEL.

•	Select the appropriate LD analysis tool. TASSEL offers various LD statistics such as r^2, D', etc.

•	Choose the parameters for LD analysis, including the window size, minimum minor allele frequency threshold, and others as per your study requirements.

**5.	Run Analysis:**

•	Start the LD analysis by clicking on the appropriate button or menu option.

•	TASSEL will calculate LD statistics for pairs of markers across your dataset according to the specified parameters.

**6.	Extract LD File:**

•	Once the LD analysis is complete, you can typically save the results in various formats including text files.

•	Look for options within the TASSEL interface to save the LD results. This may involve selecting a file format and specifying a location to save the file.

**7.	Interpret Results:**

•	Once you have the LD file extracted, you can analyze it using statistical software such as R. Code has been provided.

•	LD analysis results will typically include measures of LD such as r^2 or D' between pairs of markers, which can be interpreted to understand the genetic linkage patterns in your dataset.
