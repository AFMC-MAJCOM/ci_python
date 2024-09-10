# ci_python
A generic continuous integration pipeline for python projects.

## Using a reusable workflow in your repository workflow
Reusable workflows, found in the .github/workflows folder, can only be called directly in a job and not in steps. Below is an example of how you can use the ci.yaml reusable workflow in your own workflow yaml file. There are two inputs required, the project_root which is just the root name of the project the workflow will be called in and then the mark_inputs. This is the marks used to label all your pytests and will check that all pytests are marked. For example if the projects pytests are labeled with unit, integration and end2end your mark_inputs string should look like this: "not unit and not integration and not end2end"
```
jobs:
  generic-workflow:
    uses: AFMC-MAJCOM/ci_python/.github/workflows/ci.yaml@main
    with:
      project_root: <your_project>
      mark_inputs: <your_marks>
```

### Using composite actions with your repository workflow
Composite actions, found in subfolders in the .github/actions folder can be called in steps of a job in your workflow. If your workflow is going to run pytests, you can use our pytest-setup action instead of setting it up on your own, as shown:
```
jobs:
  run-pytests:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Run pytest setup
      uses: AFMC-MAJCOM/ci_python/.github/actions/pytest-setup@main
    - name: Run Pytests
      run: pytest -v
```
### Installing Optional Dependencies
By default, these workflows will run `pip install .` for linting and pytest runners. If your repository has optional dependencies that need to be installed when running these tests, you will need to include them in the `optional_dependencies` input. For example, if you have the optional dependencies `api` and `tests`, you will use:
```
jobs:
  generic-workflow:
    uses: AFMC-MAJCOM/ci_python/.github/workflows/ci.yaml@main
    with:
      optional_dependencies: '[api,tests]'
      ...
```
If your runners do not have optional dependencies, you do not need to use this input.