# Experiment 2: Continuous Integration for Python Using Jenkins Freestyle

## Aim
To implement continuous integration for a Python application using GitHub and Jenkins Freestyle, with automatic source checkout, unit testing, and application execution.

## Software Requirements
Windows, Python 3, Git, GitHub account, Jenkins with Git plugin, web browser.

## Program: `app.py`

```python
def greet():
    return "Hello DevOps"

if __name__ == "__main__":
    print(greet())
```

## Test Program: `test_app.py`

```python
import unittest
from app import greet

class TestApp(unittest.TestCase):
    def test_greet(self):
        self.assertEqual(greet(), "Hello DevOps")

if __name__ == "__main__":
    unittest.main()
```

## Procedure
1. Create `Python-Freestyle-CI` folder with `app.py` and `test_app.py`.
2. Open terminal in the folder. Run `python app.py` and `python -m unittest discover -v`.
3. Create an empty GitHub repository and push the code using `git init`, `git add .`, `git commit`, `git branch -M main`, `git remote add origin ...`, and `git push -u origin main`.
4. Open Jenkins and create **New Item > Freestyle project** named `Python-Freestyle-CI`.
5. In **Source Code Management**, select **Git**, enter the repository URL and `*/main` branch.
6. (Optional) Enable **Poll SCM** with schedule `* * * * *`.
7. Add build step **Execute Windows batch command**:

```bat
python -m unittest discover -v || exit /b 1
python app.py || exit /b 1
```

8. (Optional) Archive artifacts `app.py,test_app.py`.
9. Save, select **Build Now**, and open **Console Output**.

## Expected Output

```text
test_greet (test_app.TestApp.test_greet) ... ok
----------------------------------------------------------------------
Ran 1 test in 0.000s
OK
Hello DevOps
Finished: SUCCESS
```

## Result
Thus, the Python application was integrated with GitHub and Jenkins Freestyle for automatic checkout, unit testing, and execution. A successful Jenkins build displays `Finished: SUCCESS`.
