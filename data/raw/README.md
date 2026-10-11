# Input data

The following user-supplied input files are included, unchanged:

| File | Required format | Used by |
| --- | --- | --- |
| `sp500_raw.xlsx` | Two columns, date and S&P 500 price; loaded with `header=None`. Text prices containing commas are cleaned by the notebooks. | Both notebooks |
| `ibm.csv` | Columns `Date` and `Adj Close`. Daily observations are filtered to 1962–2009 and the first available observation in each month is selected. | Bivariate notebook |

Record the provider, retrieval date, price convention, and redistribution terms before distributing data. The supplied notebooks do not establish these details. Only the two named input files are included by the Git ignore rules; other raw data files are ignored.

Validation: both notebooks construct 576 monthly prices and 575 monthly returns over the intended sample. The aligned return period is February 1962 to December 2009. `ibm(1).csv` was renamed to `ibm.csv` without changing its contents.
