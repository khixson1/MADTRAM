# MADTRAM
MS/MS-Annotator-Data-TRansformer_Adduct_Assigner (MADTRAM) for machine learning applications

![image](https://user-images.githubusercontent.com/8357088/167742737-9eb36d46-e431-456e-a29c-4177308506c4.png)
![image](https://user-images.githubusercontent.com/8357088/167742794-61d776ee-52d0-4df3-b073-12315bbb699d.png)

This script was created and tested in Google's Colaboratory Python environment (https://colab.research.google.com). It takes a single ms/ms text output file obtained from processing a mass spectrometer raw file first through ProteomeWizard's MSConvert(https://proteowizard.sourceforge.io/download.html) software. This script takes MS/MS text results for a single compound/standard and reorganizes it to display binned relative intensities for each mass (range = 50-3500). Pubchem metadata and Classyfire annotations are added by just entry of either the compound name or CAS number. Selected Ions are mapped to possible adduct/ion losses for ions that are 1+, 2+ and 3+. The output is a single row containing over 8000 dimensions of information for each MS/MS scan. Output provides a simple .csv file which can be combined with other output .csv files for other compounds which altogether can be used for subsequent machine learning analysis. A project report is also produced to show Total Ion Chromatogram (TIC), Selected Ion Chromatogram (SIC), prominant ion counts and prominant ion cutoff as well as a summary table showing selected ion masses and adduct matches.

This code takes text input files which were created from ProteoWizard 3.0 (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4324452/) (https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4113728/), a free software which is capable of converting most raw datafiles from most mass spectrometry companies into a datafile amenable to universal downstream analyses (e.g., .txt, .mxml, .msmxml). In the provided example a ThermoScientific .RAW file was converted into into a text file using MSConvert centroid peak peaking and charge state correction applied during the RAW-to-text file conversion.

Data Transformation Steps:

I. Working directory and text file name and location are specificText (.txt).

II. User-defined metadata (e.g., compound name and CAS) is manually entered.

III. Optional user-defined information can be modified (e.g., predicted ion losses)

IV. Metadata from MS analysis text file and pubchem (https://pubchem.ncbi.nlm.nih.gov/) and classyfire (http://classyfire.wishartlab.com/about) are extracted and added to each scan metadata output.

V. mz extracted from ms/ms scans are rounded to nearest dalton.

VI. Intensity data from ms/ms is scaled to % of highest peak, peaks below 1% are filtered out, and the data are then further binned into 10 bins (1-10, 11-20, 21-30, 31-40, 41-50, 51-60, 61-70, 71-80, 81-90, 91-100 %).

VII. Columns are created to represent every m/z considered between m/z 50-3500. This algorithm is meant for small molecule ms/ms data with charge state 1+, 2+ or 3+ possibilities.

VIII. m/z columns are populated with % bin if an intensity for that ms/ms scan is found at that m/z. NaN values are denoted by 0.

IX. m/z columns are then concatenated to the user-defined and txt-file-defined metadata columns.

X. Probable ESI adduct m/z are calculated and matched to selected ion m/z to putatively assign adduct identity. Filter set at +/- 0.01 Da for adduct calculation.

XI. Data scans are further filtered out based on 4 criteria: 1)most prominant ions are determined based on count and scans not containing these ions are removed. 2) m/z that is below a 4+ charge state are removed. 3) if an adduct is not identified for the selected ion m/z, scan is removed, and 4) if the predicted adduct contains atoms that are not in chemical formula of the compound analyzed, the scan is removed.

XII. Data table representing each ms/ms scan of probable intensities at each m/z value along with corresponding metadata are represented by each row.
Data table is saved as a .csv file to defined working directory folder.

XIII. Analysis report showing total ion chromatogram, selected ion chromatogram, an elbow plot showing cutoff of prominant/characteristic ions used for selected ms/ms scans and a summary adduct table are saved to the working directory.

