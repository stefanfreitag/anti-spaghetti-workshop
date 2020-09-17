<!-- markdownlint-disable MD012 -->

# Anti Spaghetti Workshop



## What this is about

- ~~Finger pointing~~ <!-- .element: class="fragment" data-fragment-index="1" -->
- ~~Blaming~~  <!-- .element: class="fragment" data-fragment-index="1" -->

- Completely biased observations<!-- .element: class="fragment" data-fragment-index="2" -->
- Food for thoughts <!-- .element: class="fragment" data-fragment-index="3" -->



## Starting point

- "Storage Report" project on business side <!-- .element: class="fragment" data-fragment-index="1" -->
- Files and folders belonging to the project <!-- .element: class="fragment" data-fragment-index="2" -->
- Basic Python knowledge on my side <!-- .element: class="fragment" data-fragment-index="3" -->
- No one talking me through the code <!-- .element: class="fragment" data-fragment-index="4" -->

Note:

- Working on existing project with no hand over


## Observations

Note:

- A "pattern" I noticed and one or two examples
- Do you find yourself in those pattern?



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

<img src="../images/patryk-gradys-4pPzKfd6BEg-unsplash.jpg" height="300px">


Implemented by creating files and directories as shown in observation 1

```sh
./Chesters Trial
./TEST

./Chesters Trial/DataReader/EntsogReader.py
./Chesters Trial/DataReader/EntsogReaderOLD.py

```

- Possible reasons
  - Accidentally deletion of files
  - Refactoring
- Differences between copy and original file increase over time
- In the IT world "TEST" would contain e.g. unit tests (later more)



### Observation 3

>Where to start?

<img src="../images/muhammad-haikal-sjukri-1NzJggtJ6j4-unsplash.jpg" height="300px">


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
ModuleNotFoundError: No module named 'pandas'
```


Batch files are fine for Windows, but do not work under Linux

```sh
❯ ./pyCleaningDataContractsTrigger.bat
./pyCleaningDataContractsTrigger.bat: 1: call: not found
: not foundgDataContractsTrigger.bat: 2:
: not foundgDataContractsTrigger.bat: 3: R:
./pyCleaningDataContractsTrigger.bat: 4: cd: can't cd to R:GDMAD-BALLGEMEINDatenaustauschStorage
': [Errno 2] No such file or directoryontracts.py
: not foundgDataContractsTrigger.bat: 6:
: not foundgDataContractsTrigger.bat: 7: pause
```



#### Observation 4

>No dependency management

<img src="../images/xavi-cabrera-kn-UmDZQDjM-unsplash.jpg" height="300px">


Providing source code is not sufficient

- Used libraries need to be available on target  _or_
- creation of a distribution<br>e.g. `setuptools`

- Concept of virtual environments<br>e.g. `requirements.txt`



#### Observation 5

>Remove useless content and files

<img src="../images/markus-spiske-JDFmHZpzJBw-unsplash.jpg" height="300px">


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


<img src="../images/perfect_example.png" height="500px">



#### Observation 6

>Avoid re-inventing the wheel

<img src="../images/jon-cartagena-mmf7olkmhfw-unsplash.jpg" height="300px">


`GetDataAPI.py`

```python
def unixtime_to_datetime(unixtime):
  """Return datetime (UTC)for a given unixtime.
  Parameters
  ----------
  unixtime: int
  The unixtime to convert to datetime.
  Returns
  -------
  datetime: datetime.datetime
  The datetime (UTC) corresponding to the given unixtime.
  """
  dt = datetime.datetime(1970, 1, 1)
       + datetime.timedelta(0, unixtime)
  return dt
```


Built-in function

```python
dt = datetime.fromtimestamp(unixtime, tz=timezone.utc)
```


Related questions

- Documentation: `unixtime` in s or ms?

- What is `0` and `unixtime` mapped to in `timedelta`?

```python
datetime.timedelta(days=0, seconds=unixtime)
```



#### Observation 7

>Missing error handling & logging

<img src="../images/esther-wechsler-Ty3C3cIRhug-unsplash.jpg" height="300px">


```sh
$ python3 Get_Report.py
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



#### Observation 8

>No hardcoded endpoints and passwords

<img src="../images/anita-jankovic-KGbX1f3Uxtg-unsplash.jpg" height="300px">


- `Helper_Function_Storage_Report.py`<br>Database user and password for SMART1P

- `Smart Reader.py`<br>Database user for SMART


- `Modena Reader.py`<br>CAO Gas CE  RDS PostgreSQL<br>Modena Database, user `eit_adm`

- `MadbaseReader.py`<br>MODNLYTC@GDM1D


- `MAD_Price_Loader.py`<br>Madbase@ORCL3P  



### Recommendations



#### Recommendation 1

- Store code & assets under version control (git)
- Use tools like Bitbucket



#### Recommendation 2

- Keep your repository clean
  - Use a `.gitignore` file
  - Content defines what _not_ enters the repository

- Examples:
  - `__pycache__`
  - `pyc` files
  - log files



#### Recommendation 3

Manage dependencies
(virtual environments)


Using virtual environments

```sh
python3 -m venv /home/stefan/python/venv/anti_spaghetti
source /home/stefan/python/venv/anti_spaghetti/bin/activate
```


Installation and freezing requirements

```sh
pip3 install  <dependency>

pip3 freeze > requirements.txt
```


Later on another system, e.g.

```sh
pip3 install -r requirements.txt
```



#### Recommendation 4

- Apply tools IT likes
  - code formatter
  - code linter
  


Linter output on `GetDataAPI.py`

- trailing-whitespace
- bad-indentation
- unused-variable
- unused-import
- invalid-name
- ungrouped-imports
- wrong-import-order


Linter on `EntsogReader.py`

```sh
Your code has been rated at -15.29/10
```



#### Recommendation 5

Take your decision wisely


Some libraries have dependencies on the OS

> xlwings requires an installation of Excel and therefore only works on Windows and macOS.


- Moving the Python code e.g. to Cloud or Linux boxes could get complicated
- Can we use `pyexcel`, `openpyxl` or just `csv`?



## Lunchtime over

<img src="../images/richard-bell-NXHwphKj1Yc-unsplash.jpg" height="300px">



## Links

- [Bitbucket](https://bitbucket.org/product/)
- [Black](https://github.com/psf/black)
- [git](https://git-scm.com/)
- [GitKraken](https://www.gitkraken.com/)
- [How To Package Your Python Code](https://python-packaging.readthedocs.io/en/latest/)
- [Pylint](https://github.com/PyCQA/pylint)



## Backup slides



### Outlook


#### Unit testing


unittest

- Included in the Python standard library
- API will be familiar to anyone who has used JUnit/nUnit/CppUnit
- Gives confidence to not break existing functionality


Creating test cases is accomplished by subclassing unittest.TestCase

```python
import unittest

def fun(x):
    return x + 1

class MyTest(unittest.TestCase):
    def test(self):
        self.assertEqual(fun(3), 4)
```

(Source: https://docs.python-guide.org/writing/tests/)


Example: MeritO User Sight Transformer

``` python
 def test_convert_to_report_strategic_sales(self):
        with open("strategic_sales.xml", "r") as file:
            text = file.read()
            sights = converter.extract_sights(text)
            owner = Owner(name="Backoffice", uuid=str(uuid.uuid4()))
            reports = converter.convert_to_report(sights, owner)
            report = next(iter(reports))
            self.assertCountEqual(
                {
                    "a920b071-f91c-478f-bc47-0a9a72e4638c-thirdparty_delivery_obo_evonik_engienew",
                    "a920b071-f91c-478f-bc47-0a9a72e4638c-thirdparty_delivery_obo_evonik_wingas",
                    "6f455429-0020-4ce2-89df-865e1a0b20b3-balancingEnergy",
                    "1bb97746-541a-43d8-8e91-cd3b69ddafac-balancingEnergy",
                },
                map(lambda item: item.time_series_id, report.items),
            )
```


#### Python & remote execution

- Python code often executed on local machine
  - Desktop, not always on
  - limited access by others (restarting service etc) 
- Options for execution on remote systems
  - Containers
  - Cloud
  - Serverless



Hello World

```python
from flask import Flask

server = Flask(__name__)

@server.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    server.run(host="0.0.0.0")
```



Dockerfile

```python
# first stage
FROM python:3.8 AS builder
COPY requirements.txt .

# install dependencies to the local user directory (eg. /root/.local)
RUN pip install --user -r requirements.txt

# second unnamed stage
FROM python:3.8-slim
WORKDIR /code

# copy only the dependencies installation from the 1st stage image
COPY --from=builder /root/.local/ /root/.local
COPY ./src .

# update PATH environment variable
ENV PATH=/root/.local/bin:$PATH

CMD [ "python", "./server.py" ]
```


Creating a Docker image and running a container 

```sh
docker build -t flask-hello-world .


docker run -d -p 5000:5000 flask-hello-world:latest
0c362cfed041d0580386e0b7ef24c18317a378a8319744d38f8b81ddf82d3c02
```





### Example EntsogReader.py

```sh
pylint score

> Your code has been rated at -15.69/10
```


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

```sh
$ black  --target-version=py37 EntsogReader.py

Your code has been rated at 3.30/10 (previous run: -16.20/10, +19.50)
```


Removing unused import

```sh
EntsogReader.py:1:0: W0611: Unused import os (unused-import)
EntsogReader.py:6:0: W0611: Unused import cx_Oracle (unused-import)
```


Fixing import order

```sh
EntsogReader.py:6:0: C0411: standard import "import datetime" should be placed before "from DataReader.AbstractDataReader import AbstractDataReader" (wrong-import-order)
EntsogReader.py:7:0: C0411: third party import "import dateutil.parser" should be placed before "from DataReader.AbstractDataReader import AbstractDataReader" (wrong-import-order)
EntsogReader.py:8:0: C0411: third party import "import requests" should be placed before "from DataReader.AbstractDataReader import AbstractDataReader" (wrong-import-order)
EntsogReader.py:9:0: C0411: third party import "import pandas as pd" should be placed before "from DataReader.AbstractDataReader import AbstractDataReader" (wrong-import-order)
```


When executing

```sh
Cannot compare tz-naive and tz-aware datetime-like objects
```
