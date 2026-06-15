from veracode_api_py import Applications, SBOM
import time
from datetime import datetime, timedelta, timezone
from pathlib import Path
import json

BASE_SBOM_DIRECTORY = "sboms"
BASE_DATE = datetime.now(tz=timezone.utc) - timedelta(weeks=55)
SBOM_FORMAT = "cyclonedx"

def try_get_all_apps(attempt=1):
    try:
        return Applications().get_all()
    except Exception as e:
        print(f"Error getting applications (attempt {attempt}): {e}")
        if attempt < 3:
            time.sleep(2 ** attempt)  # Exponential backoff
            return try_get_all_apps(attempt + 1)
        else:
            print("Failed to get applications after 3 attempts.")
            return []
        
def try_get_sbom(application_guid, attempt=1):
    try:
        return SBOM().get(application_guid, format=SBOM_FORMAT)
    except Exception as e:
        print(f"Error getting SBOM for application GUID {application_guid} (attempt {attempt}): {e}")
        if attempt < 3:
            time.sleep(2 ** attempt)  # Exponential backoff
            return try_get_sbom(application_guid, attempt + 1)
        else:
            print(f"Failed to get SBOM for application GUID {application_guid} after 3 attempts.")
            return None

def save_sbom(application_name, sbom):
    filename = f"{application_name.replace("/", "_").replace("\\", "_").replace(" ", "_")}_sbom.json"
    with open(f"{BASE_SBOM_DIRECTORY}/{filename}", "w") as file:
        file.write(json.dumps(sbom))

def is_last_13_months(modified_date):
    return modified_date and datetime.fromisoformat(modified_date) >= BASE_DATE

def has_scan(application):
    scans = application["scans"]
    return scans and [scan for scan in scans if scan["scan_type"] == "STATIC" and is_last_13_months(scan["modified_date"])]

def main():
    Path(BASE_SBOM_DIRECTORY).mkdir(parents=True, exist_ok=True)
    applications = try_get_all_apps()

    # Loop through each application and get its SBOM
    for application in applications:
        application_guid = application['guid']
        if has_scan(application):
            print(f"Getting SBOM for application GUID: {application_guid}")
            sbom = try_get_sbom(application_guid)
            if sbom:
                print(f"Saving SBOM for application GUID: {application_guid}")
                save_sbom(application["profile"]["name"], sbom)
        else:
            print(f"Skipping application {application_guid} as it has no SBOM to fetch")

if __name__ == "__main__":
    main()
