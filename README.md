# Watermark GUI

GUI to watermark your pictures with text and/or another picture.

Installation:

```bash
python3 -m pip install watermark
```

## Hacking

```bash
# Setup the virtualenv
python3 -m venv ~/venv3
. ~/venv3/bin/activate

# Setup pre-commit for automatic code quality checks before commit
python -m pip install -U --user pre-commit
pre-commit install
```

## Testing

```bash
python -m pip install -U --user tox
tox
```
