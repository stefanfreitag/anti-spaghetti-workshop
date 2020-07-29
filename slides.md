<!-- markdownlint-disable MD012 -->

# Anti Spaghetti Workshop


## Starting point

- Limited Python expertise on my side
- No one was talking me through the code
- What is the input?
  - Files and folders in "Storage Report" folder



## Observations



### Observation 1

>Multiple versions of same file in different folders

<img src ="../images/agnieszka-kowalczyk-c0VRNWVEjOA-unsplash.jpg" height="300px">


#### Identical naming

Example 1

```sh
./CleaningDataContracts.py
./Chesters Trial/CleaningDataContracts.py
```

Example 2

```sh
./CleaningData.py
./TEST/CleaningData.py
```


#### (Copy of) Copies

Example

```sh
./New Storage Report.twb
./New Storage Report - Copy.twb
```


#### "OLD" Versions

Example 1

```sh
./Chesters Trial/EntsogReader.py
./Chesters Trial/EntsogReaderOLD.py
```

Example 2

```sh
./Chesters Trial/DataReader/EntsogReader.py
./Chesters Trial/DataReader/EntsogReaderOLD.py
```



### Observation 2

> Pseudo-version control system

<img src ="../images/patryk-gradys-4pPzKfd6BEg-unsplash.jpg" height="300px">


Implemented by creating files and directories as shown in observation 1

```sh
./Chesters Trial
./TEST

./Chesters Trial/DataReader/EntsogReader.py
./Chesters Trial/DataReader/EntsogReaderOLD.py

```



(in IT "TEST" would contain unit tests to execute against the "product")



### Observation 3

>Where to start?


Python files

```sh
./NewStorageReportPython.py
./Get_Report.py
./StorageReportFromExtract.py
```

Batch files

```sh
./pyCleaningDataContractsTrigger.bat  
./pyCleaningDataTrigger.bat
```


Independent of the Python file I use as an entry point, errors pop up.

```sh
$ python3 NewStorageReportPython.py
Traceback (most recent call last):
  File "NewStorageReportPython.py", line 1, in <module>
    import xlwings as xw
ModuleNotFoundError: No module named 'xlwings'
```

```sh
$ python3 Get_Report.py
Traceback (most recent call last):
  File "Get_Report.py", line 1, in <module>
    import pandas as pd
ModuleNotFoundError: No module named 'pandas
```



#### Observation 4

>No dependency management


- Providing source code is not sufficient
- Used libraries need to be installed on target
- Concept of virtual environments / requirements.txt
- Eases life when moving from local machine to server



#### Observation 5

>Remove useless content and files


`Prism_Engine.py`

no meaningful content/ 72 blank lines


`Helper_Function_Storage_Report.py`

commented out code<br>
46/95 rows, 48%


`Cleaning Data.py`<br>contains code that is two times commented out

```python
#
# # import shutil
# #
# # dst_dir = "R:\\GD\MAD-B\\ALLGEMEIN\\04 STO\\Tableau Projects\\Storage Report Project\\New Storage Report\\"
# # now = str(dt.datetime.now())[:19]
# # ...
```

`PerfectExample.py`


Missing `texture.png` file<br>
Is the code required for the report?



#### Observation 6

>Encapsulation


```python
def API_to_DataFrame(url):
    def unixtime_to_datetime(unixtime):
        """Return datetime (UTC)for a given unixtime.
        Parameters
        ----------
        unixtime: int
        The unixtime to convert to datetime.◘
        Returns
        -------
        datetime: datetime.datetime
        The datetime (UTC) corresponding to the given unixtime.
        """
        dt = datetime.datetime(1970, 1, 1) + datetime.timedelta(0, unixtime)
        return dt
```



#### Observation 7

>Avoid re-inventing the wheel


In `GetDataAPI.py` we have

```python
def unixtime_to_datetime(unixtime):
        """Return datetime (UTC)for a given unixtime.
        Parameters
        ----------
        unixtime: int
        The unixtime to convert to datetime.◘
        Returns
        -------
        datetime: datetime.datetime
        The datetime (UTC) corresponding to the given unixtime.
        """
        dt = datetime.datetime(1970, 1, 1) + datetime.timedelta(0, unixtime)
        return dt
```


```python
dt = datetime.datetime.fromtimestamp(0, tz=timezone.utc)
```

- Unix time in seconds or milliseconds? ms gives error
- Sticking to ISO format?



#### Observation 8

Missing error handling & logging


```shell
❯ python3 Get_Report.py
Traceback (most recent call last):
  File "/usr/lib/python3.8/urllib/request.py", line 1326, in do_open
    h.request(req.get_method(), req.selector, req.data, headers,
  File "/usr/lib/python3.8/http/client.py", line 1240, in request
    self._send_request(method, url, body, headers, encode_chunked)
...
  File "/usr/lib/python3.8/socket.py", line 918, in getaddrinfo
    for res in _socket.getaddrinfo(host, port, family, type, proto, flags):
socket.gaierror: [Errno -2] Name or service not known
```

### Remove unused code


- 'BackTester.py: Invalid indentation in line 1
- `Get_Report.py: Is it used at all?


Cleaning Data.py

```python
# # import shutil
# #
# # dst_dir = "R:\\GD\MAD-B\\ALLGEMEIN\\04 STO\\Tableau Projects\\Storage Report Project\\New Storage Report\\"
# # now = str(dt.datetime.now())[:19]
# # now = '\\' + now.replace(":", "_")
# #
# # filename_dst1 = str(now) + r"Clean_Data.xlsx"
# # dst_file1_dir = dst_dir + filename_dst1
# #
# # shutil.copy(wb_write_out, dst_file1_dir)
# #
#
# # then solve UTC problem, then total problem then AGSI problem then perhaps write looping into class
# # injection and withdrawal curve change only limited to today now
# # injection and withdrawal curve change does not accumulate?
# pass
```



## Recommendations

- Store code & assets (required files) under version control
  - git/ Stash/ Bitbucket
  - gitignore file for __pycache__
- Add a README.md
  - Details on installation/ usage/ dependencies
- Replacing "print(xyz)" by usage of logger
- Using virtual environments

```sh
python3 -m venv /home/stefan/python/venv/anti_spaghetti
source /home/stefan/python/venv/anti_spaghetti/bin/activate
```

- Installation and freezing requirements

```sh
pip3 install  <dependency>
pip3 freeze > requirements.txt
```

Later on another system:

```sh
pip3 install -r requirements.txt
```


- Use IT tools like
  - code linter
  - (static) code analysis


Linter on GetDataAPI.py:

- trailing-whitespace
- bad-indentation
- unused-variable
- unused-import
- invalid-name
- ungrouped-imports
- wrong-import-order


Linter on EntsogReader.py:
```sh
Your code has been rated at -15.29/10
```

ToDo:

- Python Naming of methods
- black: Black is a really useful code formatting tool maintained by the Python Software Foundation. It reformats files to improve
readability
- Propose improvements
- "xlwings requires an installation of Excel and therefore only works on Windows and macOS. To enable the installation on Linux nevertheless, do: export INSTALL_ON_LINUX=1; pip install xlwings" => can we use another library to make it work on Linux (pyexcel, openpyxl)
- Can we use csv instead of Excel?


- CleaningData.py: Separation of logical blocks by ###########################################
- What does `PerfectExample.py` do?
  - File `texture.png` is missing
  - `AttributeError: module 'time' has no attribute 'clock'`, deprecated since Python 3.3?



## Example EntsogReader.py


pylint score

> Your code has been rated at -15.69/10


Constructor Comment

```python
"""base class to get data from SMART"""
```


"Empty" constructor

```python
 def __init__(self):
       pass
```


Commented out code
(Indentation)

```python
   def getUrl(self,entry, exit , indicator,periodtype, start_date, end_date):
            #exit = self.configuration.at[ticker,'POINT_EXIT']
            #entry = self.configuration.at[ticker,'POINT_EXIT']
            #indicator=self.configuration.at[ticker,'INDICATOR']
            result = 'https://transparency.entsog.eu/api/v1/operationalData?forceDownload=true&pointDirection='
```


Running  black


❯ black  --target-version=py37 EntsogReader.py

Your code has been rated at 3.30/10 (previous run: -16.20/10, +19.50)


Removing unused import

EntsogReader.py:1:0: W0611: Unused import os (unused-import)
EntsogReader.py:6:0: W0611: Unused import cx_Oracle (unused-import)


Fixing import order

EntsogReader.py:6:0: C0411: standard import "import datetime" should be placed before "from DataReader.AbstractDataReader import AbstractDataReader" (wrong-import-order)
EntsogReader.py:7:0: C0411: third party import "import dateutil.parser" should be placed before "from DataReader.AbstractDataReader import AbstractDataReader" (wrong-import-order)
EntsogReader.py:8:0: C0411: third party import "import requests" should be placed before "from DataReader.AbstractDataReader import AbstractDataReader" (wrong-import-order)
EntsogReader.py:9:0: C0411: third party import "import pandas as pd" should be placed before "from DataReader.AbstractDataReader import AbstractDataReader" (wrong-import-order)

- EntsogReader:
  - When executing
```shell
Cannot compare tz-naive and tz-aware datetime-like objects
```
- Removing unused imports
- Remove default constructor
- Removing commented out code
- Fixing bad-whitspace

  - Wrong comments

```python
class EntsogReader(AbstractDataReader):
    """base class to get data from SMART"""
    def __init__(self):
       pass
```


- OptiReader:

```python
def main():
    pass


if __name__ == "__main__":
    main()
```


## Longest spaghetti (Measuring code quality)

## (Testing)

## From spaghetti to penne (Refactoring)


