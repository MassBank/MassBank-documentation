# Contributor documentation

## How to contribute to MassBank? 
Contributors are welcome to provide new spectra to MassBank. New spectra should meet the format described in the [MassBank record format specification](https://github.com/MassBank/MassBank-documentation/blob/dev/MassBankRecordFormat.md). All MassBank spectra are stored in the [MassBank-data](https://github.com/MassBank/MassBank-data) repository. To submit new spectra create a pull request to MassBank-data or open an issue to get in contact.

### Generation of new records for upload to MassBank-data
The preferred software to generate MassBank records is [RMassBank](https://www.bioconductor.org/packages/release/bioc/html/RMassBank.html). RMassBank is in continuous development. Hence, use the latest version available, please. If you are using your own pipeline please consider to provide it to other users(please open an issue).

### Validate new record files locally
Before new record files are accepted to MassBank-data, they need to pass the validation. This will be done automatically if a pull request is created. It's also possible to validate record files locally in advance. To validate new record files locally please download the latest release from the [MassBank-cli-tools](https://github.com/MassBank/MassBank-cli-tools) repo and run it.

The release contains the compiled jar files in the lib folder and a litle helper script for convinience in the bin folder. 
The `Validator` supports validation of a single record file, several files as well as one or several directories containing record files, eg. 
```
./bin/Validator <my record directory> 
```

### Submit new records to MassBank-data or modify existing records
To submit data or to change existing records:
1. Please fork the [MassBank-data](https://github.com/MassBank/MassBank-data) repo, clone it to your place and make your changes or add new content in the dev branch.
2. Commit and push your changes to the dev branch of your repo.
3. Create a pull request against the dev branch.

Your pull request will be validated and you will receive a report whether your records meet the MassBank record format specification. If everything is ok, your content will be integrated into MassBank.

If you are not able to provide your contribution via a pull request, please fill a issue at the [issue board](https://github.com/MassBank/MassBank-data/issues) or get in contact via email.

## FAQ
### Access and workspace creation
- Can a user create a workspace and upload data independently?
- Are there specific requirements a user has to meet before being allowed to publish data?

The user cannot upload data independently. The MassBank data is located in a public GitHub repository and hence new datasets can be contributed via pull requests (PR). The contributor needs to have a GitHub account, create a fork of the [MassBank-data](https://github.com/MassBank/MassBank-data) repository and do a PR. The MassBank record format and requirements can be found in the [Documentation](https://github.com/MassBank/MassBank-documentation/blob/dev/MassBankRecordFormat.md).
### Data review and curation
- Are the data reviewed by your team or another responsible person before publication?
- Is there any content or quality check/curation?

After the PR, an automatic validation routine will check the datasets for containing the mandatory fields and the right formats for mandatory and optional fields within every single MassBank record. To use the validation tool locally, there are MassBank’s [CLI tools](https://github.com/MassBank/MassBank-cli-tools). For single record file validation, the user can also use the integrated validation on the [MassBank service web page](https://massbank.eu/MassBank/service). The [documentation](https://massbank.eu/MassBank/documentation) can be found there as well. 

### Handling of unsuitable data
- What happens if, for example, a user uploads data that is unsuitable or cannot be successfully validated?
- Is there a review or moderation process to identify and, if necessary, remove or modify such data?

If the validation routine fails, the validation routine will point to the file causing the first validation error. Based on that and if needed with help and guidance from the MassBank team, the user can modify the incoming data of that PR and re-do the PR after the necessary changes. 

### Responsibility for the data
- Who is responsible for the published data: The user/research group or the repository?
- Is this responsibility defined in any kind of agreement or policy?

The MassBank team maintains the MassBank-data repository. Hence, any data modification is taken seriously and only allowed if the validation passed. After merging new data from contributors into the MassBank data, the user is able to modify files via another PR, if necessary. MassBank is an open-source and freely accessible software with open and public data too.

### Roles and permissions
There are no specific roles within MassBank. Comments on issues or PRs are possible with a GitHub account. The MassBank team is the maintainer of the software as well as the data.
