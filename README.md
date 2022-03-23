# VarFish Workflow: Mass Queries

This Snakemake-based workflow allows mass queries that even works with large VarFish projects.

## Installing

As a prerequisite, install conda and `mamba` package.
Activate `conda`.

```terminal
$ mamba env create -n varfish-wf-queries --file environment.yaml
$ conda activte varfish-wf-queries
```


## Configuring

First of all, ensure that `~/.varfishrc.toml` is properly configured, e.g.

```toml
[global]
varfish_server_url = "https://varfish.example.com"
varfish_api_token = "1234567890123456789012345678901234567890123456789012345678901234"
```

Then, copy `config/main.yml.example` to `config/main.yml`.
Adjust the settings:

- `varfish_shortcuts`
    - `query_presets` - the query presets, chose one of `de_novo`, `dominant`, `recessive`, `mitochondrial`
- `varfish_projects` list of
    - `name` - name of the project for folders
    - `uuid` - UUID of project in VarFish
- `genes` - optional; list of gene symbols (or entrez/ensembl IDs) to restrict the search for - `null` for no restriction
- `max_cases` - opional; number of cases to limit query to - `null` for no restriction

## Running

```
# snakemake --configfile=config/main.yml --cores 10 -d workflow
```

## Tricks

If anything gets stuck, use the following to for re-running queries:

```terminal
$ for x in $(find workflow/translate-namse -name query.json); do y=${x/.json/_result.json}; test -e $y || rm -f $x; done
```
