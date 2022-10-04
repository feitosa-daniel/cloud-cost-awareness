# Supplementary Material

This repository contains the supplementary material for the paper entitled:

**Mining Cloud Cost Awareness — Is it possible?**

This repository contains two files:
- `repositories.json`
- `dataset.json`

> **Long-term storage**
> 
> This is a temporary repository for double-blind review. If the submission is accepted, we will upload its content to an archival repository that guarantees long-time storage and update the manuscript with the DOI.

## Metadata

### **`repositories.json`**

- **Content**: Identifier and summary metrics of the repositories that contain evidence of cost awareness in cloud-based software.
- **Format**: List of entries in JSON.
- **Size**: 434 entries.
- **Collection period**: May 2022.
- **Entry fields** ([JSON Schema](https://json-schema.org/)):
  ```json
  {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "title": "RepositoryInfo",
    "type": "object",
    "properties": {
      "name": {
        "type": "string",
        "description": "Name of the GitHub repository (i.e., 'user/repository-name')."
      },
      "ncommits": {
        "type": "integer",
        "description": "Total number of commits in the repository at the time of collection.",
        "minimum": 0
      },
      "nauthors": {
        "type": "integer",
        "description": "Total number of commits in the repository at the time of collection.",
        "minimum": 0
      },
      "ninfrasloc": {
        "type": "integer",
        "description": "Total number of source lines of code (SLOC) in Terraform descriptor files (i.e., deployment infrastructure as code, IaC) at the time of collection.",
        "minimum": 0
      },
      "nsloc": {
        "type": "integer",
        "description": "Total SLOC in files other than Terraform descriptors at the time of collection.",
        "minimum": 0
      }
    }
  }
  ```
- Example entry:
  ```json
  {
      "name": "AJarombek/global-aws-infrastructure",
      "ncommits": 197,
      "nauthors": 3,
      "ninfrasloc": 3736,
      "nsloc": 2503
  }
  ```

### **`dataset.json`**

- **Content**: Evidence of cost awareness in commits of cloud-based software repositories.
- **Format**: List of entries in JSON.
- **Size**: 538 entries.
- **Collection period**: May 2022.
- **Entry fields** ([JSON Schema](https://json-schema.org/)):
  ```json
  {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "title": "RepositoryInfo",
    "type": "object",
    "properties": {
        "url": {
            "type": "string",
            "description": "Link to the commit."
        },
        "content": {
            "type": "string",
            "description": "Commit message."
        },
        "codes": {
            "type": "array",
            "items": {
                "type": "string",
                "description": "Cost-related code assigned to the commit message."
            }
        }
    }
  }
  ```
- Example entry:
  ```json
  {
    "url": "https://github.com/alphagov/govuk-aws/commit/5fa5da9756f12559b490217dd5b173db48e7f2a9",
    "content": "Resize graphite machine type\n\nUpdate machine type to m5.xlarge. It should be cheaper, we tried to\nresize it before but it didn't work because of disk labels. Trying again\nafter the 'Device' tag was added to the EBS volume.",
    "codes": ["saving","instance"]
  },
  ```

## License

The data compiled in this repository is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International]
(https://creativecommons.org/licenses/by-nc/4.0/) (CC BY-NC 4.0) License.


