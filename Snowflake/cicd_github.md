# Code Main Branch
## Files
* One DDL per file
* One DML per table per file

## Initial Commit Deployment
* N Files
* Deploy all N Files

## Option 1: Task Commit Branches
* M Files
* Deploy all M Files
* M Files =  Select ALL FILES where file change timestamp > Sprint Start Date
* .gitignore (Manual add files to ignore)

## Option 2: Task Commit Branches
* Deploy List File (Manual Creation)
* Execute all files in the order they appear in the `Deploy List File`

