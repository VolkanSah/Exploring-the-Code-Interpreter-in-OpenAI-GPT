# The Code Execution Environment in OpenAI GPT 

#### (version 11.12.2025)

The code execution environment in OpenAI GPT models provides an interactive Python sandbox.
It allows running Python code, performing analysis, generating files, and working with data directly inside ChatGPT or within custom GPTs.
While subject to sandbox and security limits, it is a highly flexible tool for learning, prototyping, testing, and exploring Python-based workflows.

## Table of Contents

* What is the Code Execution Environment?
* What is it used for?
* How can ChatGPT help with programming?
* Limitations
* Benefits
* Installation and Setup (local use)
* Data Storage

  * Detailed explanation of `/mnt/data`
* Working with Images
* Working with Excel Files

  * Advanced Excel Processing
* Working with Word Files

  * Advanced Word Processing
* Working with PDF Files

  * Advanced PDF Processing
* Other advanced applications
* Contributing
* Credits

---

## What is the Code Execution Environment?

The code execution environment (formerly known as the “Code Interpreter”) is a secure Python sandbox embedded inside GPT models.
It runs Python code, processes files, and returns results directly to the user.

## What is it used for?

A wide range of tasks, including:

* Mathematical computations
* Data analysis and data transformation
* Prototyping and debugging Python
* Working with files: images, documents, spreadsheets, PDFs
* Teaching and learning programming
* Automating small workflows

## How can ChatGPT assist with programming?

ChatGPT can:

* Generate code
* Review and debug existing code
* Explain concepts
* Create structured workflows
* Help with refactoring, analysis, or documentation

---

## Limitations

The sandbox has several constraints:

* No internet access
* No access to the host operating system
* Runtime limit around 300 seconds
* Memory limits depending on the model
* Only the directory `/mnt/data` is readable and writable
* The environment resets between sessions

GPTs (not plain ChatGPT sessions) may also include **persistent file storage**, but this is separate from the runtime sandbox.

---

## Benefits

* Safe execution environment
* Immediate feedback loop
* Supports a wide spectrum of Python libraries
* Ideal for experiments, tutorials, and fast prototyping
* No risk to the user’s local system

---

## Installation and Setup (Local)

You only need this if you want to reproduce the examples locally.
ChatGPT users do **not** need to install anything.

```shell
pip install pandas openpyxl python-docx pypdf fpdf2 matplotlib pillow
```

Notes:

* `pypdf` is the modern replacement for `PyPDF2`
* Many of these libraries are already available inside ChatGPT’s sandbox

---

## Data Storage

The Python sandbox has access to the directory `/mnt/data`.
Files placed there can be read, written, and downloaded.

### Detailed explanation of `/mnt/data`

`/mnt/data` is a temporary storage area that:

* Persists across code cells
* Does **not** persist after the session ends
* Supports reading and writing of any file type

Examples:

**Writing a text file**

```python
with open('/mnt/data/numbers.txt', 'w') as file:
    for num in range(10):
        file.write(str(num) + '\n')
```

**Reading it**

```python
with open('/mnt/data/numbers.txt', 'r') as file:
    numbers = file.readlines()
```

**Saving a plot**

```python
import matplotlib.pyplot as plt

plt.plot([0, 1, 2], [0, 1, 4])
plt.savefig('/mnt/data/plot.png')
```

---

## Working with Images

Examples using PIL and matplotlib:

Display:

```python
from PIL import Image
import matplotlib.pyplot as plt

img = Image.open('/mnt/data/your_image.jpg')
plt.imshow(img)
plt.axis('off')
plt.show()
```

Resize:

```python
img_resized = img.resize((800, 600))
```

Rotate:

```python
img_rotated = img.rotate(90)
```

Convert to grayscale:

```python
img_gray = img.convert('L')
```

---

## Working with Excel Files

Reading:

```python
import pandas as pd
df = pd.read_excel('/mnt/data/example.xlsx')
print(df.head())
```

Writing:

```python
df.to_excel('/mnt/data/output.xlsx', index=False)
```

Filtering:

```python
filtered = df[df['Age'] > 30]
```

Sorting:

```python
sorted_df = df.sort_values('Age')
```

### Advanced Excel Processing

Pivot tables:

```python
pivot = df.pivot_table(index='Category', values='Sales', aggfunc='sum')
```

Merging multiple Excel files:

```python
import glob
files = glob.glob('/mnt/data/*.xlsx')
dfs = [pd.read_excel(f) for f in files]
merged = pd.concat(dfs, ignore_index=True)
```

---

## Working with Word Files

Reading:

```python
from docx import Document
doc = Document('/mnt/data/example.docx')

for para in doc.paragraphs:
    print(para.text)
```

Writing:

```python
doc = Document()
doc.add_paragraph('Hello world')
doc.save('/mnt/data/new.docx')
```

Tables:

```python
table = doc.add_table(rows=3, cols=3)
```

---

## Working with PDF Files

Reading text:

```python
from pypdf import PdfReader

reader = PdfReader('/mnt/data/example.pdf')
text = reader.pages[0].extract_text()
print(text)
```

Writing PDFs:

```python
from fpdf import FPDF

pdf = FPDF()
pdf.add_page()
pdf.set_font("Arial", size=12)
pdf.cell(200, 10, txt="Hello PDF", ln=True)
pdf.output('/mnt/data/new.pdf')
```

### Advanced PDF Processing

Merging:

```python
from pypdf import PdfMerger

merger = PdfMerger()
merger.append('/mnt/data/file1.pdf')
merger.append('/mnt/data/file2.pdf')
merger.write('/mnt/data/merged.pdf')
merger.close()
```

Splitting:

```python
reader = PdfReader('/mnt/data/example.pdf')

for i, page in enumerate(reader.pages):
    writer = PdfWriter()
    writer.add_page(page)
    with open(f'/mnt/data/page_{i+1}.pdf', 'wb') as f:
        writer.write(f)
```

---

## Contributing

Contributions and improvements are welcome.
Pull requests and issues are appreciated.

---

## Credits
- Volkan Kücükbudak
- OpenAI GPT models for interactive assistance
- Community contributors
- Repository: [https://github.com/VolkanSah/The-Code-Interpreter-in-OpenAI-GPT/](https://github.com/VolkanSah/The-Code-Interpreter-in-OpenAI-GPT/)

> updated 11.12.2025

