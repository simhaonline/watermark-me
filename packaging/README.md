# Deployment Script

We are using scripts to automate the isolated environment creation. With only one script, you will be able to setup the environment, launch the tests suite and build the final package.

You could modify these scripts, but we will not be able to do support, these are the official way to manage the application ecosystem.

Finally, scripts names are referring to Jenkins, but you can always execute them on your personal computer or outside a Jenkins job.

## GNU/Linux, macOS

### Usage

    sh packaging/$OSI/deploy.sh [ARG]

Where `$OSI` is one of: `linux`, `osx`.

Possible `ARG`:

    --build: freeze the client into self-hosted binary package
    --check-upgrade: check the auto-update works
    --install: install a complete development environment
    --check: check AppImage conformity (GNU/Linux only)
    --install-python: install only Python
    --install-release: install a complete environment for a release (without tests requirements)
    --start: start Nuxeo Drive
    --tests: launch the tests suite

Executing the script without argument will setup/update the isolated environment.

### Dependencies:

See [pyenv](https://github.com/yyuu/pyenv/wiki/Common-build-problems#requirements) requirements.

## Windows

**PowerShell 4.0 or above** is required to run this script. You can find installation instructions [here](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-windows-powershell).

### Usage

    powershell .\packaging\windows\deploy.ps1 [ARG] [-direct]

Possible `ARG`:

    -build: freeze the client into self-hosted binary package
    -check_upgrade: check the auto-update works
    -install: install a complete development environment
    -install_release: install a complete environment for a release (without tests requirements)
    -start: start Nuxeo Drive
    -tests: launch the tests suite

Executing the script without argument will setup.update the isolated environment.

### Dependencies:

[//]: # (XXX_PYTHON, XXX_INNO_SETUP)

- [Python 3.8.1](https://www.python.org/ftp/python/3.8.1/python-3.8.1.exe).
- [Inno Setup 6.0.2](http://www.jrsoftware.org/isdl.php) to create the installer.

### Troubleshooting

If you get an error message complaining about the lack of signature for this script, you can disable that security check with the following command inside PowerShell (as Administrator):

	set-executionpolicy -executionpolicy unrestricted

## Environment Variables

### Required Envars

- `APP_NAME_SNAKE` is the full application name in snake case.
- `APP_NAME_DIST` is the basename of the PyInstaller spec file, without the extension.
- `REPOSITORY_NAME` is the current folder name containing sources.
- `WORKSPACE` is the **absolute path** to the source folder.

### Optional Envars

- `PYTHON_VERSION` is the required **Python version** to use, i.e. `3.8.1`.
- `SPECIFIC_TEST` is a **specific test** to launch. The syntax must be the same as [pytest markers](http://doc.pytest.org/en/latest/example/markers.html#selecting-tests-based-on-their-node-id), i.e.:
```
    test_local_client.py (an entire test file)
    test_local_client.py::TestLocalClient (a whole class)
    test_local_client.py::TestLocalClient::test_make_documents (only one method)
```
- `SKIP` is used to tweak tests checks:
```
    - SKIP=flake8 to skip code style
    - SKIP=mypy to skip type annotations
    - SKIP=cleanup to skip dead code checks
    - SKIP=rerun to not rerun failed test(s)
    - SKIP=integration to not run integration tests on Windows
    - SKIP=all to skip all above (equivalent to flake8,mypy,rerun,integration)
    - SKIP=tests tu run only code checks
```

#### MacOS Specific

Those are related to code-signing:
- `KEYCHAIN_PATH` is the **full path** to the certificate.
- `KEYCHAIN_PWD` is the **password** to unlock the certificate.

#### Windows specific

[//]: # (XXX_INNO_SETUP)

- `APP_NAME` is the **application name** used for code sign, i.e. `Nuxeo Drive`.
- `ISCC_PATH` is the **Inno Setup path** to use, i.e. `C:\Program Files (x86)\Inno Setup 6`.
- `PYTHON_DIR` is the **Python path** to use, i.e. `C:\Python37-32`.
- `SIGNTOOL_PATH` is the **SignTool path** to use, i.e. `C:\Program Files (x86)\Windows Kits\10\App Certification Kit`.
