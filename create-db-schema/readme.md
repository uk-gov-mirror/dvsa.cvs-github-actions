# Create DB Schema

Triggers a workflow in another repository to create a database schema with access to the target database. The action uploads the local database configuration, invokes the specified workflow, waits for completion, and reports the results in the workflow summary.

## Inputs

This action requires the following inputs:

* environment: Target environment where the schema should be created.
* schema: Name of the schema to create.
* database: Database connection identifier.
* drop-schema: (optional) Drop the existing schema before recreating it - defaults to `false`.
* repository: (optional) Repository containing the workflow to execute - defaults to `cvs-devops`.
* branch: (optional) Branch to run the workflow from.
* workflow: (optional) Workflow file to execute - defaults to `create-db-schema.yaml`.
* dry-run: (optional) Perform a dry run without making changes - defaults to `false`.

## Outputs

This action produces the following outputs:

* schema: The schema name that was processed.

## Notes

* This action requires GitHub CLI (`gh`) to be available on the runner.
* The target workflow must support workflow dispatch and accept the expected input payload.
* Any database configuration files located in the `db/` directory are uploaded as an artifact and made available to the triggered workflow.
* The action waits for the triggered workflow to complete before continuing.
* The action will fail if the triggered workflow does not complete successfully.

## Usage Example

```yaml
...
jobs:
  create-schema:
    runs-on: [runner]

    steps:
      - name: Create Database Schema
        id: schema
        uses: dvsa/cvs-github-actions/create-db-schema@develop
        with:
          environment: test
          schema: my_schema
          database: postgres-main
          drop-schema: 'true'
          repository: cvs-devops
          workflow: create-db-schema.yaml
          dry-run: 'false'

      - name: Display Outputs
        run: |
          echo "Schema: ${{ steps.schema.outputs.schema }}"
```
