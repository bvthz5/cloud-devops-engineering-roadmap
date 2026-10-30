# 03 - Building Production CLI Tools: argparse and click

## 1. Using `click` for Modern CLIs

`click` provides intuitive decorators for building production command-line interfaces:

```python
import click

@click.group()
def cli():
    """Kubernetes & Cloud Cluster Maintenance CLI."""
    pass

@cli.command()
@click.option("--namespace", "-n", default="default", help="Kubernetes namespace")
@click.option("--dry-run", is_flag=True, help="Simulate actions without deleting")
def clean_failed_pods(namespace: str, dry_run: bool):
    """Delete all pods in Evicted or Failed state."""
    click.echo(f"Scanning namespace: {namespace} (Dry-run: {dry_run})")
    # Remediation logic here...

if __name__ == "__main__":
    cli()
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Subprocess Execution and System Management](./02-Subprocess-Execution-and-System-Management.md) | [Index](../../../README.md) | [04 - REST API Automation requests and httpx →](./04-REST-API-Automation-requests-and-httpx.md) |
