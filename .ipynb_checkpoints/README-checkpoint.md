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

### Using composite ations with your repository workflow
Composite actions can be called in steps of a job in your workflow. If your workflow is going to run pytests, you can use our pytest-setup action instead of setting it up on your own, as shown:
```
jobs:
  run-pytests:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Run pytest setup
      uses: AFMC-MAJCOM/ci_python/.github/actions/pytest-setup
    - name: Run Pytests
      run: pytest -v
```
