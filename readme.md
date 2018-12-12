# Step by step process

## 1. Fetch the organisation IDs

From the 3 files that Stephen provided, extract charity & company numbers and
create list of unique organisation identifiers (in the org-id style).

The normal guidance is to prioritise Company over Charity numbers but in this
case we use charity number if present - there is more charity data available
and charity account PDFs are already OCRd.

Save the resulting list of org identifiers as a CSV file.

- in `1-get-org-to-check.ipynb`

## 2. Download PDF accounts

### Company accounts

For accounts of companies use Companies House API to download the document.
If an IXBRL account is available then get that, otherwise get the PDF.



- in `2-download-account-docs.ipynb`

## Convert PDF to PBM files

```
pdfimages -j test.pdf test_image
```

## List all pbm files in directory

Windows only:

```
dir *.pbm /b /a-d > filelist.txt
```

(produce unix/mac equivalent, or cross-platform python)

## OCR the image files

```
tesseract filelist.txt test_output pdf
```

C:\Users\drkan\AppData\Local\Programs\Python\Python36\Scripts\pdf2txt.py

## Transform the PDF into an HTML file

```
(python) pdf2txt.py -s 0.25 -L 0.1 -F 0.1 -o test_output.html test_output.pdf
```

For the options:
 - `-s 0.25` scales the HTML file to 25% of the size (the original is quite big)
 - `-L 0.1` sets the Line height needed to trigger a new line to smaller than the default
 - `-F 0.1` sets the parser to prioritise horizontal position of boxes

(see <https://euske.github.io/pdfminer/index.html>)