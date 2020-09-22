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

![image info](../images/agnieszka-kowalczyk-c0VRNWVEjOA-unsplash.jpg)<!-- .element height="375px" -->


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

![image info](../images/patryk-gradys-4pPzKfd6BEg-unsplash.jpg)<!-- .element height="375px" -->


Implemented by creating files and directories as shown in observation 1

```sh
./Chesters Trial
./TEST

./Chesters Trial/DataReader/EntsogReader.py
./Chesters Trial/DataReader/EntsogReaderOLD.py

```


- Possible reasons
  - Backup in case of accidental deletion
  - Refactoring
- Differences between copy and original file increase over time

Note:

- In the IT world "TEST" would contain e.g. unit tests (later more)



### Observation 3

>Where to start?

![image info](../images/muhammad-haikal-sjukri-1NzJggtJ6j4-unsplash.jpg)<!-- .element height="375px" -->


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


For all Python files I tried as entry point

errors popped up. <!-- .element: class="fragment" data-fragment-index="2" -->


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


Batch files are fine when running Windows,

but do not work under Linux<!-- .element: class="fragment" data-fragment-index="2" -->

![image info](../images/tux.png)<!-- .element: class="fragment" data-fragment-index="2" height="300xp"-->


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

![image info](../images/xavi-cabrera-kn-UmDZQDjM-unsplash.jpg)<!-- .element height="375px" -->


Providing source code is not sufficient

- Used libraries need to be available on target
- creation of a distribution
- Concept of virtual environments



#### Observation 5

>Remove useless content and files

![image info](../images/markus-spiske-JDFmHZpzJBw-unsplash.jpg)<!-- .element height="375px" -->


Partially related to observation 1 and 2


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


![image info](../images/perfect_example.png)<!-- .element height="500px" -->


&rarr; No!



#### Observation 6

>Avoid re-inventing the wheel

![image info](../images/jon-cartagena-mmf7olkmhfw-unsplash.jpg)<!-- .element height="375px" -->


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


Python docs:

```plain
classmethod datetime.fromtimestamp(timestamp, tz=None)¶
```

Return the local date and time corresponding to the POSIX timestamp, [...].

If optional argument `tz` is `None` or not specified, the timestamp is converted to the platform’s local date and time, and the returned datetime object is naive.


Related questions

- Documentation: `unixtime` in s or ms?

- What is `0` and `unixtime` mapped to in `timedelta`?

```python
datetime.timedelta(days=0, seconds=unixtime)
```



#### Observation 7

>Missing error handling & logging

![image info](../images/esther-wechsler-Ty3C3cIRhug-unsplash.jpg)<!-- .element height="375px" -->


In `Get_Report.py` a client

- connects to a remote server and
- fetches data for further processing


If the server is not reachable this is returned

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


Difficult to understand what failed. Helpful details

- host
- endpoint
- (parameter)



#### Observation 8

>No hardcoded endpoints and passwords

![image info](../images/anita-jankovic-KGbX1f3Uxtg-unsplash.jpg)<!-- .element height="375px" -->


- `Helper_Function_Storage_Report.py`<br>Database user and password for SMART1P

- `Smart Reader.py`<br>Database user for SMART


- `Modena Reader.py`<br>CAO Gas CE  RDS PostgreSQL<br>Modena Database, user `eit_adm`

- `MadbaseReader.py`<br>MODNLYTC@GDM1D


- `MAD_Price_Loader.py`<br>Madbase@ORCL3P  



### Recommendations



#### Recommendation 1

- Store code & assets under version control (git)
- Use tools like Bitbucket
- If access is missing &rarr; please ask IT



#### Recommendation 2

- Keep your repository clean
  - Use a `.gitignore` file
  - Content defines what _not_ enters the repository<br>Examples: `__pycache__`, `pyc` & log files



#### Recommendation 3

Manage dependencies
(virtual environments)


Use case

- Application require
  - packages and modules that don’t come as part of the standard library
  - sometimes a specific version of a library (fix/ feature)

&#8623; One Python installation may not be able to meet the requirements of every application


Virtual environment

> self-contained directory tree that contains a Python installation for a particular version of Python, plus a number of additional packages.

- Different applications can then use different virtual environments


Using virtual environments

Creation

```sh
python3 -m venv /home/stefan/python/venv/anti_spaghetti
```

Activation

```sh
source /home/stefan/python/venv/anti_spaghetti/bin/activate
```


Installing and freezing requirements

```sh
pip3 install pandas numpy keras

pip3 freeze > requirements.txt
```


Content of `requirements.txt`

```plain
cx-Oracle==8.0.0
DateTime==4.3
numpy==1.19.0
pandas==1.0.5
pyglet==1.5.7
requests==2.24.0
...

```



#### Recommendation 4

Apply tools likes

- code formatter
- code linter
  
(A linter is a small program that checks code for stylistic or programming errors)


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


Running  black

```sh
$ black  --target-version=py37 EntsogReader.py

Your code has been rated at 3.30/10 (previous run: -16.20/10, +19.50)
```



#### Recommendation 5

Take your decision wisely


Some libraries have dependencies on the OS

> xlwings requires an installation of Excel and therefore only works on Windows and macOS.


- Moving Python code e.g. to Cloud or Linux boxes could get complicated
- Could we use `pyexcel`, `openpyxl` or just `csv` files?



#### Recommendation 6

Test your code


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



## Lunchtime over

![image info](../images/richard-bell-NXHwphKj1Yc-unsplash.jpg)<!-- .element height="375px" -->



## Links

- [Bitbucket](https://bitbucket.org/product/)
- [Black](https://github.com/psf/black)
- [git](https://git-scm.com/)
- [GitKraken](https://www.gitkraken.com/)
- [How To Package Your Python Code](https://python-packaging.readthedocs.io/en/latest/)
- [Pylint](https://github.com/PyCQA/pylint)



## Backup slides



### Outlook




#### Python & remote execution

- Python code often executed on local machine
  - not always powered on
  - limited access by others
  - scheduled execution of code "challenging"
- Options for execution on remote systems
  - Server/ EC2 instances
  - Containerized
  - Serverless (Lambda functions)
  


Hello World - Containerized

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


Creating a Docker image

```sh
docker build -t flask-hello-world .
```


Starting a container

```sh
docker run -d -p 5000:5000 flask-hello-world:latest
0c362cfed041d0580386e0b7ef24c18317a378a8319744d38f8b81ddf82d3c02
```


Sending data to the container

```sh
curl -F 'file=@8712842.decoded' -i http://localhost:5000/converter\?owner_uuid\=123
```
