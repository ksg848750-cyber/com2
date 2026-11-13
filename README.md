# Experiment 2: Python CI using Jenkins Freestyle and GitHub

## Aim
Implement Continuous Integration for a Python application using GitHub and Jenkins Freestyle, with automatic source code checkout, testing, and execution.

## Requirements
- Python 3 installed and on PATH
- Git installed
- Jenkins running on Windows (often http://localhost:8080 or http://localhost:8081)
- Jenkins Git plugin and a GitHub repository

## Project files
- `app.py` — prints `Hello DevOps` and exposes `greet()` for testing.
- `test_app.py` — one unit test using Python's built-in `unittest` module.
- `README.md` — these execution instructions.
- `WRITEUP.md` — exam-ready lab record.

## Step 1 — Run and test locally in CMD or PowerShell
Extract the ZIP, open a terminal in `Python-Freestyle-CI`, and run:

```cmd
python --version
python app.py
python -m unittest discover -v
```

Expected output from `python app.py`:

```text
Hello DevOps
```

Expected test output (timing varies):

```text
test_greet (test_app.TestApp.test_greet) ... ok

----------------------------------------------------------------------
Ran 1 test in 0.000s

OK
```

If `python` is not recognized, install Python, tick **Add Python to PATH**, and reopen the terminal.

## Step 2 — Push the project to GitHub
Create an **empty** repository on GitHub, for example `Python-Freestyle-CI` (do not add README, license or .gitignore on GitHub). From the project folder run:

```cmd
git init
git add .
git commit -m "Add Python app and unit test"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/Python-Freestyle-CI.git
git push -u origin main
```

Replace `YOUR_USERNAME` with your real GitHub username. GitHub sign-in may be required. If Git complains about identity, configure `git config --global user.name "Your Name"` and `git config --global user.email "you@example.com"` and retry the commit.

## Step 3 — Create a Jenkins Freestyle project
1. Open Jenkins in a browser (use your configured port, e.g. `http://localhost:8081`).
2. Select **New Item**.
3. Enter `Python-Freestyle-CI`, choose **Freestyle project**, and click **OK**.
4. Under **Source Code Management**, select **Git**.
5. Set **Repository URL** to `https://github.com/YOUR_USERNAME/Python-Freestyle-CI.git`.
6. Set branch to `*/main`. Supply credentials if the repository is private.
7. Optional: under **Build Triggers**, enable **Poll SCM** and enter `* * * * *` (poll every minute; it triggers a build only when a change is detected).
8. Under **Build Steps**, choose **Add build step > Execute Windows batch command**.
9. Paste the batch commands below. They run the tests first; `|| exit /b 1` fails the Jenkins build if testing fails.

```bat
python -m unittest discover -v || exit /b 1
python app.py || exit /b 1
```

10. Optional: under **Post-build Actions**, select **Archive the artifacts** and enter `app.py,test_app.py` (archives source files; it is *not* an application package).
11. Click **Save**, then **Build Now**.
12. Click the build number > **Console Output**.

## Step 4 — Verify Jenkins console output
A successful build contains test output and application output similar to:

```text
test_greet (test_app.TestApp.test_greet) ... ok
----------------------------------------------------------------------
Ran 1 test in 0.000s
OK
Hello DevOps
Finished: SUCCESS
```

Your build may show other Git checkout and Jenkins log messages before and after these lines.

## Step 5 — Demonstrate continuous integration
Change the greeting consistently in `app.py` and `test_app.py`, then execute:

```cmd
git add .
git commit -m "Update Python greeting"
git push
```

Click **Build Now** again, or wait for **Poll SCM** (if enabled). Jenkins checks out the changed GitHub files, tests them, and runs the app.

## Notes
- No `pip install` or `requirements.txt` needed: the unit test uses built-in Python `unittest`.
- This is a **Jenkins Freestyle** project, not Pipeline. There is **no Jenkinsfile**.
- This experiment tests and executes a console application. It does not perform a deployment or produce a distributable Python package.
