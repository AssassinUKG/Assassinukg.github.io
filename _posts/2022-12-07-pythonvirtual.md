---
title: Python Virtual Enviroment (with specific python version) 
date: 2022-12-04 16:35:00 +00:00
categories: [tutorial, code, python]
tags: [tutorial, code, python]     # TAG names should always be lowercase
---



1. Open your terminal and navigate to the directory where you want to create the virtual environment.  
2. Run the following command to create a virtual environment named "venv" with the specific version of Python:  

```bash
python3 -m venv venv --python=python3.7
```

3. Activate the virtual environment by running the following command:  

```bash
source venv/bin/activate
```

4. Verify that the virtual environment is active by checking the version of Python:  

```bash
python --version
```

5. You should see the specific version of Python (e.g. Python 3.7) in the output.  

6. To deactivate the virtual environment, run the following command:  

```bash
deactivate
```

7. You can now use this virtual environment to install and run Python packages without affecting the system Python installation.


