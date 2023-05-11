# pytest

This action will set up python, install requirements, and then run pytest with code coverage capture turned on and reported in XML format.
The test result and code coverage files will be uploaded as an artifact called `test-reports` and can be used by subsequent jobs/steps.
Pytest is invoked via `pytest --cov -n logical` to support code coverage capture and running tests in parallel.

This action will use a `.coveragerc` (or `pyproject.toml`, `setup.cfg`, or `tox.ini`) in the root directory of the calling repository to configure coverage running and reporting.
Information in the configuration file will define the set of files that will have coverage information captured.
See https://coverage.readthedocs.io/en/6.3.2/source.html for more information about source file configuration.
An example `.coveragerc` is included in this directory to be used if wanted.

## Example

```yaml
# This template is intended to be incorporated as a workflow in another repository to gate pull requests
# behind passing results.
name: Run pytest

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  run-unit-tests:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [ '3.7', '3.10' ]
    steps:
      - name: Check out repository code
        uses: actions/checkout@v3
      - name: Run pytest
        uses: open-reading-frame/pytest-action@main
        with:
          python-version: ${{ matrix.python-version }}
          requirements-txt: path/to/python/requirements.txt
```
