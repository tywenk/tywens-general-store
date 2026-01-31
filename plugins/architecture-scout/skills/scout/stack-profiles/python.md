# Python Stack Profile

## Import Patterns
- Relative imports (`from . import`) vs absolute — check consistency
- Star imports (`from module import *`) — hides dependencies
- Circular import workarounds (imports inside functions) — structural issue
- Check `__init__.py` files — empty vs re-exporting vs containing logic

## Package Manager
- `requirements.txt` → pip (check for pinned vs unpinned versions)
- `pyproject.toml` with `[tool.poetry]` → Poetry
- `Pipfile` → pipenv
- `setup.py` / `setup.cfg` → traditional packaging
- `conda` environment files → data science stack
- Multiple systems = migration in progress

## Framework Signals
- `manage.py` → Django (check version in requirements)
- Flask/FastAPI/Starlette → check for consistent routing patterns
- `celery` → async task processing
- SQLAlchemy / Django ORM / Tortoise → ORM choice

## Legacy Indicators
- Python 2 syntax (`print` statements, `unicode` type, `xrange`)
- Missing type hints → older codebase or data science code
- `os.path` alongside `pathlib` → incomplete migration
- `unittest` style alongside `pytest` → test framework migration
- String formatting: `%` vs `.format()` vs f-strings → era indicator
- `six` library → Python 2/3 compatibility layer still present

## Configuration Surfaces
- `pyproject.toml` (consolidates tool config)
- `setup.cfg` / `setup.py`
- `tox.ini` / `noxfile.py`
- `mypy.ini` / `pyproject.toml [tool.mypy]`
- `.flake8` / `ruff.toml`
- `.env` files
- `alembic.ini` / Django migrations

## Testing Conventions
- `tests/` directory vs `test_*.py` co-located
- pytest vs unittest — check for migration
- Fixtures vs setUp/tearDown patterns
- Check for test factories, conftest.py organization
- Coverage configuration

## Common Migration Patterns
- Python 2 → 3: Look for `__future__` imports, `six` usage
- Django version upgrades: Check deprecated features in templates/views
- Flask → FastAPI: Look for both frameworks' routing patterns
- unittest → pytest: Look for mixed test styles
- Untyped → typed: Look for partial type hint coverage, `py.typed` marker
