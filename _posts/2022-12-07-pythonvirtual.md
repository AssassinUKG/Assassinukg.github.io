---
title: Python Virtual Enviroment (with specific python version) 
date: 2022-12-07 18:25:00 +00:00
categories: [tutorial, code, python]
tags: [tutorial, code, python]     # TAG names should always be lowercase
---



To create a Python 3 virtual environment with a specific version of Python, follow these steps:

  1. Install the virtualenv package using pip: `pip install virtualenv`
  2. Create a new virtual environment with the desired Python version by specifying the version number   
  with the `-p` flag, like this: `virtualenv -p /usr/bin/python3.6 my_env`
  4. Activate the virtual environment by running the following command: `source my_env/bin/activate`

Once your virtual environment is activated, you can begin installing packages and using Python with the specific version you selected.   

When you are finished working in the virtual environment, you can deactivate it by running the `deactivate` command.
