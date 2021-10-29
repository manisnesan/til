# CLI using Typer

Typer allows to build awesome command line interface(cli) based applications. It is inspired by pydantic, argparse, click etc.

Sample Code demonstrating cli arguments, cli options with default values

```python
import typer


def main(
    name: str=typer.Argument("World", help="The name of the user to greet", show_default=True),#CLI argument with Help
    lastname: str = typer.Option("...", help="Last name of the person to greet", prompt=True),
    formal: bool = typer.Option(False, help="Say hi formally")): #CLI Options with Help
    """
    Say hi to NAME, optionally with a --lastname

    If --formal is used, say hi very formally
    """
    if formal:
        typer.echo(f"Good day Mr. {name} {lastname}")
    else:
        typer.echo(f'Hello {name} {lastname}')


if __name__ == '__main__':
    typer.run(main)
```

## CLI application with multiple commands

```python
import typer

app = typer.Typer()


@app.command()
def create():
    typer.echo("Creating user: Mani")


@app.command()
def delete():
    typer.echo("Deleting User")


if __name__ == "__main__":
    app()
```

## Typer CLI

- Provides the auto completion for the scripts without creating a separate package
- Install the typer cli `pip install typer-cli`
- `typer main.py run --help`

## Alternative

- [fastcore script](https://fastcore.fast.ai/script.html) also provides a way to convert the python functions into script.

## References

- [Typer](https://typer.tiangolo.com/)
- [Inspirations for Typer](https://typer.tiangolo.com/alternatives/)