# Guidance for migrating from Azure Databricks to Synapse Spark Analytics

> This repo contains guidance including code samples to document challenges one might face while migrating 
> from Azure Databricks to Synapse Spark and solutions to these challenges.

| Issue | Reason | Solution | Related files |
| ----- | ------ | -------- | ------------- |
| HttpException | TBF | TBF | TBF |
| Tables which needs graphframes library (no PK, UK tables) are giving error | Graphframes jar used by ADB is not the public version of library and thus differs from one used in Spark | Setup correct Graphframes jar | Code sample - Graphframes |
| Date Format Issue | Occurs when statistics are collected for the date column during checkpointing. These statistics are not used for filter file-pruning and thus can be safely switched off without impacting performance | Add spark configuration to turn off stats collection for delta | Code sample - Date Format |
| SQL Paas connection error | TBF | TBF | TBF |

**Code sample - Date Format**
```
spark.conf.set("spark.microsoft.delta.stats.collect", "false") # turning off statistics
tableExample = DeltaTable.forPath(spark, centralized_table_path)
tableExample.recomputeStatistics() # removing existing statistics, needed to execute only once
```

**Code sample - Graphframes**

*How to setup graphframes:*
1. Generated a whl file to add to the workspace following steps to generate the whl file.
2. Upload the generated whl file and the jar for same version to the spark pool.
3. Test the installation of graphframes by executing example from official docs.

*Steps to generate whl file:*
Several resources available for generating whl file like the one below:
1. Download required zip version [from here](https://spark-packages.org/package/graphframes/graphframes). Unzip setup.py available in path graphrames-zip-folder/python/setup.py
2. Modify the setup.py to have some basic info, sample setup.py at the end of the doc.
3. Install wheel: `pip install wheel setuptools`
4. Build whl file: `python path/to/setup.py bdist_wheel`. Whl file would be generated in the graphrames-zip-folder/python/dist/
5. Test that package is installed using whl file locally: `python3 -m pip install dist/graphframes-0.8.2-py3-none-any.whl`

Sample setup.py:
```
"""A setuptools based setup module.
See:
https://packaging.python.org/guides/distributing-packages-using-setuptools/
https://github.com/pypa/sampleproject
"""

# Always prefer setuptools over distutils
from setuptools import setup, find_packages
import pathlib

VERSION = '0.0.1'
DESCRIPTION = 'My first Python package'
LONG_DESCRITPTION = 'My first Python package with a slightly longer description'

setup(

    # There are some restrictions on what makes a valid project name
    # specification here:
    # https://packaging.python.org/specifications/core-metadata/#name
    name='graphframes',  # Required

    version='0.8.2',  # Required

    description='A sample Python project',  # Optional

    author='Author Name',  # Optional

    package_dir={'': 'graphframes'},  # Optional

    packages=find_packages(where='graphframes'),  # Required

    python_requires='>=3.6, <4',
    project_urls={  # Optional
        'Bug Reports': 'https://github.com/pypa/sampleproject/issues',
        'Funding': 'https://donate.pypi.org',
        'Say Thanks!': 'http://saythanks.io/to/example',
        'Source': 'https://github.com/pypa/sampleproject/',
    },
)
```


## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
