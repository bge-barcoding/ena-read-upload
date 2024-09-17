## 1_populate_tsv.py
Used to generate [ENA Tree of Life sample submisison checklist](https://www.ebi.ac.uk/ena/browser/view/ERC000053) to create sample accession numbers. Takes relevant fields from [sample_metadata.csv](https://github.com/bge-barcoding/sample-processing?tab=readme-ov-file#1_sample_processingpy) and outputs them in ToL checklist format for manual upload to ENA.

**usage: python 1_populate_tsv.py [/path/to/sample2taxid_out.csv] [/path/to/output.tsv]**
- sample_metadata.csv = Generated during sample-processing from BOLD container dowload.
- output.tsv =  Contains following fields: 'taxid', 'scientific_name', 'sample_alias', 'sample_title', 'sample_description', 'organism part', 'lifestage', 'project name', 'identified_by', 'collected_by', 'collection date', 'geographic location (country and/or eea)',
'geographic location (latitude)', 'geographic location (longitude)', 'geographic location (region and locality)', 'habitat', 'sex', 'collecting institution', 'specimen_voucher'

See 'BOLD_download-ENA_ToL_checklist_field_mapping.xlsx' for information on how fields from BOLD container downloads are used to populate required fields in ENA's Tree of Life sample registration checklist.





## 2_create_ena_submission_sheet.py
Used to generate the [sample_submission_spreadsheet](https://github.com/enasequence/ena-bulk-webincli/blob/master/example_template_read.txt) required by [ena-bulk-webincli](https://github.com/enasequence/ena-bulk-webincli) to produce the manifest file for bulk read upload to ENA. 
Takes the .csv file returned by ENA containing created sample accession numbers (and other metadata), and a path to a directory containing trimmed R1.fastq and R2.fastq files.

**usage: python 2_create_ena_submission_sheet.py path/to/sample_accession_output.csv path/to/trimmed/read/dir path/to/output_dir/output.tsv**
- path/to/sample_accession_output.csv = Output by ENA and downloaded manually from Webin account. Contains sample accession numbers and 'title' fields (i.e. Process ID).
- path/to/trimmed/read/dir = path to directory containing trimmed reads (.fastq) output by MGE or Skim2Mito (or another pipeline).
- path/to/output_dir/output.tsv = [sample_submission_spreadsheet](https://github.com/enasequence/ena-bulk-webincli/blob/master/example_template_read.txt)



## 3_ena_bulk_webincli.sh
Primarily generates manifest file for bulk upload of trimmed PE read data for upload to ENA using output.tsv from 2_create_ena_submission_sheet.py script. Requires [ena-bulk-webincli](https://github.com/enasequence/ena-bulk-webincli) to be installed in conda env.

**usage: python path/to/bulk_webincli.py -u Webin-XXXXX -p XXXXX -g reads -s path/to/sample_submission_spreadsheet.tsv	-m validate -pc 8**
- path/to/bulk_webincli.py = Path to run script supplied with ena-bulk-webincli.
- -u = Webin account username
- -p = Webin account password
- -g =Genetic context (reads, sequence, genome, transcriptome, taxrefset)
- -s = path to sample_submission_spreadhseet.tsv generated by 2_create_ena_submission_sheet.py (can be tab-separated .txt, .csv, .xlsx/.xls or .tsv)
- -m = mode (subkit/validate)
- -pc = parellel cores (between 1 and 10)
- -t = test mode for use of Webin test submission services
