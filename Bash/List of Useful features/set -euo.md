Using 'set' at the beginning of your script will allow you to setup the way your script behaves if certain things happen.
For example:
```bash
set -e    # stop script execution if a command fails
set -u    # stop script execution if variable is not set
set -o    # stop script execution if 

set -euo pipefail   # combine switches and pipefail
                    # pipefail will stop the script execution if pipe fails
```