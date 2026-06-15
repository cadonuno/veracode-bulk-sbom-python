# veracode-bulk-sbom-python

A small plugin that fetches all SBOMs available to the current user

## Purpose

This tool reads all applications available in Veracode# veracode-policy-results-to-pipeline-json

A small plugin that converts Veracode policy scan results into the equivalent pipeline scan output (for usage with Veracode Fix).

## Purpose

This tool reads Veracode policy result data and fetches their SBOM.

## Installation

1. Install required dependencies:
```bash
pip install -r requirements.txt
```

## Usage

1. Install dependencies (see Installation section above)
2. If necessary, update the script with the necessary SBOM format by changing the value of the SBOM_FORMAT constant (defaults: cyclonedx)
3. Run the plugin
```bash
py veracode-bulk-sbom-python.py
```

## Output

An SBOM json file will be generated for each application and stored at *./sboms*.
