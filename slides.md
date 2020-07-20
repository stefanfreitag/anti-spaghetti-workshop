# Anti Spaghetti Workshop


## Starting point

* Limited Python expertise on my side
* No one was talking me through the code/ functionality 


## Dude, where is my code?

* Stored on a shared drive
* Input consists only of files and folders in "Storage Report" folder


_Observation 1_

Multiple versions of same file in different folders

Identical naming

```shell
./CleaningDataContracts.py
./Chesters Trial/CleaningDataContracts.py

./CleaningData.py
./TEST/CleaningData.py
```

"Copy" is part of the file name

```shell
./New Storage Report.twb
./New Storage Report - Copy.twb
```


_Observation 2_

The project contains directories like

```shell
./Chesters Trial
./TEST
```

Missing version control system (e.g. git)

In IT "TEST" has a different meaning: contains the unit tests to execute against the "product"


_Observation 3_

With multiple Python files in place, I have to guess the application entry point

```shell
./NewStorageReportPython.py
./Get_Report.py
./StorageReportFromExtract.py
```

A README.md file is appreciated


_Observation 4_


In dependent of the file I use as an entry point, errors pop up.

```shell
❯ python3 NewStorageReportPython.py
Traceback (most recent call last):
  File "NewStorageReportPython.py", line 1, in <module>
    import xlwings as xw
ModuleNotFoundError: No module named 'xlwings'
```

```shell
❯ python3 Get_Report.py
Traceback (most recent call last):
  File "Get_Report.py", line 1, in <module>
    import pandas as pd
ModuleNotFoundError: No module named 'pandas
```


TODO: Installation of requirements
TODO: Using virtual environments


```

```shell

python3 -m venv /home/stefan/python/venv/anti_spaghetti
source /home/stefan/python/venv/anti_spaghetti/bin/activate


pip3 install  <dependency>

pip3 freeze > requirements.txt


Later on another system: pip3 install -r requirements.txt
```


_Observation 5_


Prism_Engine.py does not contain any meaningful content
Why is is part of the project?


_Observation 6_

Encapsulation  is different from
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


_Observation 7_

In GetDataAPI.py we have

``` 
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
*  Can we simply ? Show and add test
*  return instead of storing in dt
*  unixtime in seconds? As milliseconds give an error (only int is supported here, but we are passing long in case of milliseconds)
* What is returned in "unixtime_to_datetime", ISO format????
* - 2020-07-19 19:40:02
* + 2020-07-19 19:40:02+00:00






### Git (local)

### Git (remote)


## Sharing is caring (Pull requests)


## Longest spaghetti (Measuring code quality)

## (Testing)

## From spaghetti to penne (Refactoring)


### Remove unused code

Cleaning Data.py
```
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


ToDo:

- Missing README
- Propose improvements
- "xlwings requires an installation of Excel and therefore only works on Windows and macOS. To enable the installation on Linux nevertheless, do: export INSTALL_ON_LINUX=1; pip install xlwings" => can we use another library to make it work on Linux


- CleaningData.py: Separation of logical blocks by ###########################################


