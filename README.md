# Supplementary Material

This repository contains the supplementary material for the paper entitled:

**Mining Cost Awareness in the Deployments of Cloud-based Applications**

The supplementary material includes:
- scripts to support raw data collection and cached versions of data generated during the raw data collection;
- the compiled dataset containing evidence of cost awareness in commits of cloud-based software repositories, and associated information; and
- scripts to run the topic modeling steps used in the study.


**Long-term storage** 
> This is a temporary repository for double-blind review. If the submission is accepted, we will upload its content to an archival repository that guarantees long-time storage and update the manuscript with the DOI.

**Replication disclaimers:**
> (1) We note that running the provided data collection scripts from scratch will result in variations in the output files since projects have evolved and some projects may have been deleted or made private.
>
> (2) The provided scripts aid the collection of the raw data (i.e., candidate commits) to be analyzed for evidence of cost awareness (see [step4-tf-commits.json](data-collection/data/step4-tf-commits.json)). The next step to generate to provided [dataset.json](dataset.json) (i.e., coding of commits and filtering of those with relevant codes) was manual. Therefore, there is no code to automate this final step and we inly provide the dataset and associated information (i.e., [codes.json](codes.json) and [repositories.json](repositories.json)).

## Contents

### **`data-collection/`**

- **`data-collection.ipynb`** | Jupyter notebook that makes use of [PyGitHub](https://pypi.org/project/PyGithub/) and [PyDriller](https://pydriller.readthedocs.io/en/latest/commit.html) to automate the raw data retrieval, which is divided into four steps: (1) recover GitHub repositories containing HCL IaC, (2) filter repositories with Terraform files, (3) extract commits with cost-related keywords, and (4) filter commits that modify Terraform files.
- **`data/`** | Folder with the output files generated during the raw data retrieval. The files are explained in `data-collection.ipynb`. Also, you can find a cached version of each file (from the time of the study execution).
- **`Dockerfile`**, **`docker-compose.yaml`**, **`env.yaml`** | Files to build and start a Docker container with a JupyterLab instance and all necessary dependencies (see `env.yaml`).

Running scripts (via JupyterLab):
> 1. Install [Docker Engine](https://docs.docker.com/engine/install/)
> 2. In the terminal/cmd
>    1. Navigate to the folder `data-collection/`
>    2. Run `docker compose up`
> 3. In your web browser, navigate to https://localhost:8888

### **`repositories.json`**

- **Content**: Identifier and summary metrics of the repositories that contain evidence of cost awareness in cloud-based software.
- **Format**: List of entries in JSON.
- **Size**: 488 entries.
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

- **Content**: Evidence of cost awareness in commits and issues of cloud-based software repositories.
- **Format**: List of entries in JSON.
- **Size**: 746 entries (538 regarding commits and 208 regarding issues).
- **Collection period**: May 2022.
- **Entry fields** ([JSON Schema](https://json-schema.org/)):
  ```json
  {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "title": "DatasetInfo",
    "type": "object",
    "properties": {
        "type": {
            "type": "string",
            "description": "Type of entry ('commit' or 'issue')."
        },
        "url": {
            "type": "string",
            "description": "Link to the commit or issue."
        },
        "content": {
            "type": "object",
            "description": "Commit message or issue content.",
            "properties": {
              "message": {
                  "type": "string",
                  "description": "Commit message (only for commit entries)."
              },
              "title": {
                  "type": "string",
                  "description": "Issue title (only for issue entries)."
              },
              "body": {
                  "type": "string",
                  "description": "Issue body (only for issue entries)."
              },
              "comments": {
                  "type": "array",
                  "description": "Issue comments (only for issue entries).",
                  "items": {
                      "type": "string",
                      "description": "Issue comment"
                  }
              },
            }
        },
        "codes": {
            "type": "array",
            "items": {
                "type": "string",
                "description": "Cost-related code assigned to the commit message (see Section 'Codes')."
            }
        }
    }
  }
  ```
- Example entry:
  ```json
  {
    "type": "commit",
    "url": "https://github.com/alphagov/govuk-aws/commit/5fa5da9756f12559b490217dd5b173db48e7f2a9",
    "content": {
      "message": "Resize graphite machine type\n\nUpdate machine type to m5.xlarge. It should be cheaper, we tried to\nresize it before but it didn't work because of disk labels. Trying again\nafter the 'Device' tag was added to the EBS volume."
    },
    "codes": ["saving","instance"]
  }
  ```

### **`codes.json`**

- **Content**: Description of the codes identified during the study.
- **Format**: List of entries in JSON.
- **Size**: 14 entries.
- **Collection period**: May 2022.
- **Entry fields** ([JSON Schema](https://json-schema.org/)):
  ```json
  {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "title": "Code",
    "type": "object",
    "properties": {
        "name": {
            "type": "string",
            "description": "Name of the code."
        },
        "description": {
            "type": "string",
            "description": "Desription of the code."
        }
    }
  }
  ```
- Example entry:
  ```json
  {
    "name": "saving",
    "description": "denotes mentioned changes made to save costs."
  }
  ```


## Licenses

The software in this repository is licensed under the [MIT License](LICENSE).

The data compiled in this repository is licensed under the [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) (CC BY 4.0) License.
