# veracode-bulk-sbom-python

A small plugin that fetches all SBOMs available to the current user

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
   
If you have saved credentials as above you can run:
```bash
py veracode-bulk-sbom-python.py
```
Otherwise you will need to set environment variables as follows:

```
export VERACODE_API_KEY_ID=<YOUR_API_KEY_ID>
export VERACODE_API_KEY_SECRET=<YOUR_API_KEY_SECRET>
py veracode-bulk-sbom-python.py
```

## Output

An SBOM json file will be generated for each application and stored at *./sboms*.
