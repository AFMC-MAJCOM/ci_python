# ci_python
A generic continuous integration pipeline for python projects.

## Using a reusable workflow in your repository workflow
Reusable workflows, found in the .github/workflows folder, can only be called directly in a job and not in steps. Below is an example of how you can use the ci.yaml reusable workflow in your own workflow yaml file.
```
jobs:
  generic-workflow:
    uses: AFMC-MAJCOM/ci_python/.github/workflows/ci.yaml@main
    with:
      project_root: <your_project>
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

### Using markings to label pytests
Markings can be used to label pytests and control what tests the workflows run and in the order they run. Marking pytests must be done in the project directory these actions/workflows are being called from. There is a reusable workflow which enforces that all pytests in the project directory are correctly marked. The workflow expects tests to be marked with any of the following labels: "unit", "integration", and "end2end". If any tests are not marked with any of those three labels the workflow will cause the runner to fail. It can be called in your own workflow yaml file like so:
```
jobs:
  unmarked-test:
    uses: AFMC-MAJCOM/ci_python/.github/workflows/unmarked_tests.yaml@main
```