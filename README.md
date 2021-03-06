latex-mailmerge
===============

This is a very simplistic tool that takes a Latex document with Python Code embedded between \begin{python}\end{python} and a CSV or XML file and evaluates all the Python code for every line in the CSV or every <position></position> of the XML file. During the evaluation of the embedded code, a variable for every column of the CSV file is available that contains the corresponding value at the current row. In the case of XML input, a variable for every child of <position> is created.

For every row in the CSV file, respectively every <position> of the XML, the part of the Latex document between \begin{document} and \end{document} is copied with the results of the Python evaluation pasted in.

The Python snippets don't share any state between them. There are no security measures whatsoever taken to prevent a malicious document from eating your data.

I use it to generate certificates for course attendance.

Usage
-----

1. Prepare your CSV or XML. For CSV, there should be no empty lines and every column should have a header. If it's not `,`-separated, but `;`-separated (as produced by OOCalc) add the `--oocalc` switch at step 3.
2. Prepare your Latex template. It should be a normal Latex document, except you can have Python code between \begin{python} and \end{python}. In this code there are magic variables available: The headers of your columns and `_text`. Python expressions can ignore `_text`, Python statements should assign to `text`. See the `example` folder for an example template.
3. Run `python mailmerge.py template.tex data.csv`. It produces `out.tex`. XML files are recognized via the .xml extension.
4. Run (pdf)Latex on `out.tex`

Try running without arguments to get some usage information. See the examples folder for examples.

Magic Variables
---------------

In your template you can use the following magic variables:

* `_text`: Assign to this in Python statements. The code block will be replaced by the value of `_text`
* `_line`: An integer that is set to the current line of the csv.

Besides these every column header in your csv defines one variable.
