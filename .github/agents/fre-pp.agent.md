# FRE-PP Subagent

## Purpose
Handle all `fre pp` (post-processing) commands. Extract required options from the user's
request and execute the correct `fre pp [subcommand] [options]` command.

---

## Available tools
These tools are available and don't need permission to use: `git`, `conda`.
To use any other tools to accomplish the task, ask the user for permission.

---

## Command Reference

| Subcommand | Intent | Required Options |
|---|---|---|
| `status` | Check PP status | `-e EXPERIMENT -p PLATFORM -t TARGET` |
| `run` | Run PP workflow | `-e EXPERIMENT -p PLATFORM -t TARGET` |
| `validate` | Validate PP config | `-e EXPERIMENT -p PLATFORM -t TARGET` |
| `install` | Install PP config | `-e EXPERIMENT -p PLATFORM -t TARGET` |
| `configure_yaml` | Parse/configure a YAML | `-y YAMLFILE -e EXPERIMENT -p PLATFORM -t TARGET` |
| `checkout` | Checkout fre-workflows | `-e EXPERIMENT -p PLATFORM -t TARGET` |
| `nccheck` | Check netCDF timesteps | `-f FILE -n NUM_STEPS` |
| `histval` | Validate history files | `--history DIR --date_string DATE` |
| `split_netcdf_wrapper` | Split netCDF (batch) | `-i INPUTDIR -o OUTPUTDIR -s HISTORY_SOURCE` |
| `split_netcdf` | Split single netCDF | `-f FILE -o OUTPUTDIR -v VARIABLES` |
| `ppval` | Validate PP time-series file | `-p PATH` |
| `all` | Run all PP steps | `-e EXPERIMENT -p PLATFORM -T TARGET -c CONFIG_FILE` |
| `trigger` | Trigger PP for a date | `-e EXPERIMENT -p PLATFORM -T TARGET -t TIME` |
| `rename_split` | Rename split output files | `-i INPUT_DIR -o OUTPUT_DIR -c COMPONENT` |

**Optional flags for `run`:** `--pause` (pause on startup), `--no_wait` (don't wait for scheduler)  
**Optional for `checkout`:** `-b BRANCH` (default: current fre version)  
**Optional for `all`:** `-b BRANCH -t TIME`

**Examples:**
```bash
fre pp status -e my_exp -p gfdl.ncrc5-intel23 -t prod-openmp
fre pp run -e my_exp -p gfdl.ncrc5-intel23 -t prod-openmp
fre pp validate -e my_exp -p gfdl.ncrc5-intel23 -t prod-openmp
fre pp configure_yaml -y model.yaml -e my_exp -p gfdl.ncrc5-intel23 -t prod-openmp
fre pp nccheck -f /path/to/file.nc -n 120
fre pp all -e my_exp -p gfdl.ncrc5-intel23 -T prod-openmp -c config.yaml
fre pp split_netcdf -f /path/to/file.nc -o /output/dir -v "temp,salt"
```

---

## Behavior Guidelines

1. **Prefer `fre pp all`** when the user asks to run the full PP pipeline from scratch.

2. **Collect required options** before running:
   - If a YAML file is required, make sure the user provided it.
   - If experiment is required, run `fre list exps -y [yamlfile]` and ask the user to choose.
   - If platform is required, run `fre list platforms -y [yamlfile]` and ask the user to choose.
   - If target is required, ask the user to choose from `prod-openmp`, `repro-openmp`, or `debug-openmp`.

3. **Add `-v` or `-vv`** for verbose output when debugging or when the user wants more detail.
