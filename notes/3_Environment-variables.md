# Environment Variables

When you work in Python projects you probably should use a virtual environment (or a similar mechanism) to isolate the packages you install for each project.

> [!TIP]
>
> **Virtual environment ≠ Environment variable**
>
> -   **Environment variable**: variable in the system that can be used by programs.
> -   **Virtual environment**: directory with some files in it.

## First steps

### Creation and activation

-   Create a directory and enter it.
-   When you start working on a Python project for the first time, create a virtual environment inside your project. This command creates a new virtual environment in a directory called `.venv`:

    ```bash
    python -m venv .venv
    ```

    -   `python`: use the program called python
    -   `-m`: call a module as a script, we'll tell it which module next
    -   `venv`: use the module called venv that normally comes installed with Python
    -   `.venv`: create the virtual environment in the new directory .venv

-   Activate the new virtual environment so that any Python command you run or package you install uses it.

    -   When to activate:

        -   Every time you start a new terminal session to work on the project.
        -   Every time you install a new package in that environment.

        ```bash
        source .venv/bin/activate
        ```

-   We could check if the Virtual Environment is active

    ```bash
    which python /home/user/code/project/.venv/bin/python
    ```

    -   If it shows the python binary at `.venv/bin/python`, inside of your project, then it worked.

-   Once you are done working on your project you can **deactivate** the virtual environment. This way, when you run python it won't try to run it from that virtual environment with the packages installed there.

    ```py
    deactivate
    ```

### Upgrade pip

-   If you are using pip to install packages (it comes by default with Python), you should upgrade it to the latest version.
-   Make sure the virtual environment is active (with the command above) and then run:

    ```bash
    python -m pip install --upgrade pip
    ```

### Add .gitignore

-   Exclude everything in your `.venv` from Git.

    ```bash
    echo "*" > .venv/.gitignore
    ```

### Installations

-   Some packages can be installed directly

    ```bash
    pip install "fastapi[standard]"
    ```

-   Other packages could be install from `requirements.txt`. A requirements.txt with some packages could look like:

    ```txt
    fastapi[standard]==0.113.0
    pydantic==2.8.0
    ```

### Run Your Program

```bash
python main.py
```

## Why Virtual Environments

**If you just use pip directly, the packages would be installed in your global Python environment** (the global installation of Python). This is a problem, because at some point, you will probably end up writing many different programs that depend on different packages. And some of these projects you work on will depend on different versions of the same package. 😱

Now, imagine that with many other packages that all your projects depend on. That's very difficult to manage. And you would probably end up running some projects with some incompatible versions of the packages, and not knowing why something isn't working.

Also, depending on your operating system (e.g. Linux, Windows, macOS), it could have come with Python already installed. And in that case it probably had some packages pre-installed with some specific versions needed by your system. If you install packages in the global Python environment, you could end up breaking some of the programs that came with your operating system.

The solution to the problems of having all the packages in the global environment is to use a virtual environment for each project you work on. A **virtual environment** is a directory, very similar to the global one, where you can install the packages for a project. This way, each project will have its own virtual environment (.venv directory) with its own packages.

## Why Deactivate a Virtual Environment

If you don't deactivate the virtual environment for a project 'A', when you run python in the terminal, it will try to use the Python from 'A'.

But if you deactivate the virtual environment and activate the new one for 'B' then when you run python it will use the Python from the virtual environment in 'B'.
