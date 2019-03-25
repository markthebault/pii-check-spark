# Checking PII data on large datasets
This repository contains a simple PySpark notebook that **reads thought each line** of a given Spark dataframe to extract all pii values.
There is a check at two levels, the first one is at the column name level and the second one consists on checking each cell all the dataframe.

## Interpret the PII check
If the check at the column level is positive, that does not necessarily means that the dataframe contains PII data. 
In case of a cell check level positive there is a very good chance that this dataframe contains pii data

## The algorithm used
### Columns
The Pii check consist in first checking the columns, typical columns names are used for pii value (such as name, address, phone....). 
The algorithms read the column name and perform a ratio of similarity to the tipical Pii data columns names. 
If the results is superior at 0.85 percent then we consider than the column is a PII column.

### The cells
To check if a cell contains a PII value, **the algorithm runs it against some REGEX** to check the value:
- Name check
- Email
- Phone number
- Street addresses
- IPs
- Credit card number
In order to check the Names of the a person, in the python code the is a list of provided Names. 
Of course if the name is not in the list, it can not be considered as PII information.

## Limitations of this algorithm
This code is written with spark, so it scales as long as your cluster does. Checking large datasets > 1TB can be very expensive and very long. 
Note that the code is not optimized to have the maximum performances.

Currently the number of checks are not sufficient to be certain if there is no PII value undetected. 
I will advise you to add some other checks (For instance [AWS Macie](https://docs.aws.amazon.com/macie/latest/userguide/macie-classify-objects-regex.html) define those checks):
| Name                                                 | Classification | Minimum number of matches | Risk |
|------------------------------------------------------|----------------|---------------------------|------|
| Arista network configuration                         | Regex          | 1                         | 7    |
| BBVA Compass Routing Number - California             | Regex          | 1                         | 1    |
| Bank of America Routing Numbers - California         | Regex          | 10                        | 1    |
| Box Links                                            | Regex          | 1                         | 3    |
| CVE Number                                           | Regex          | 1                         | 3    |
| California Drivers License                           | Regex          | 10                        | 1    |
| Chase Routing Numbers - California                   | Regex          | 50                        | 1    |
| Cisco Router Config                                  | Regex          | 3                         | 9    |
| Citibank Routing Numbers - California                | Regex          | 1                         | 1    |
| DSA Private Key                                      | Regex          | 1                         | 8    |
| Dropbox Links                                        | Regex          | 1                         | 3    |
| EC Private Key                                       | Regex          | 1                         | 8    |
| Encrypted DSA Private Key                            | Regex          | 1                         | 3    |
| Encrypted EC Private Key                             | Regex          | 1                         | 3    |
| Encrypted Private Key                                | Regex          | 1                         | 3    |
| Encrypted PuTTY SSH DSA Key                          | Regex          | 1                         | 3    |
| Encrypted PuTTY SSH RSA Key                          | Regex          | 1                         | 3    |
| Encrypted RSA Private Key                            | Regex          | 1                         | 3    |
| Google Application Identifier                        | Regex          | 1                         | 2    |
| HIPAA PHI National Drug Code                         | Regex          | 2                         | 2    |
| Huawei config file                                   | Regex          | 1                         | 8    |
| Individual Taxpayer Identification Numbers (ITIN)    | Regex          | 100                       | 4    |
| John the Ripper                                      | Regex          | 1                         | 1    |
| KeePass 1.x CSV Passwords                            | Regex          | 1                         | 8    |
| KeePass 1.x XML Passwords                            | Regex          | 1                         | 8    |
| Large number of US Phone Numbers                     | Regex          | 100                       | 1    |
| Large number of US Zip Codes                         | Regex          | 100                       | 3    |
| Lightweight Directory Access Protocol                | Regex          | 3                         | 2    |
| Metasploit Module                                    | Regex          | 1                         | 6    |
| MySQL database dump                                  | Regex          | 1                         | 7    |
| MySQLite database dump                               | Regex          | 1                         | 7    |
| Network Proxy Auto-Config                            | Regex          | 1                         | 3    |
| Nmap Scan Report                                     | Regex          | 1                         | 7    |
| PGP Header                                           | Regex          | 1                         | 5    |
| PGP Private Key Block                                | Regex          | 1                         | 8    |
| PKCS7 Encrypted Data                                 | Regex          | 1                         | 5    |
| Password etc passwd                                  | Regex          | 4                         | 8    |
| Password etc shadow                                  | Regex          | 4                         | 8    |
| PlainText Private Key                                | Regex          | 1                         | 8    |
| PuTTY SSH DSA Key                                    | Regex          | 1                         | 8    |
| PuTTY SSH RSA Key                                    | Regex          | 1                         | 8    |
| Public Key Cryptography System (PKCS)                | Regex          | 1                         | 3    |
| Public encrypted key                                 | Regex          | 1                         | 1    |
| RSA Private Key                                      | Regex          | 1                         | 8    |
| SSL Certificate                                      | Regex          | 1                         | 3    |
| SWIFT Codes                                          | Regex          | 2                         | 4    |
| Samba Password config file                           | Regex          | 1                         | 7    |
| Simple Network Management Protocol Object Identifier | Regex          | 1                         | 5    |
| Slack 2FA Backup Codes                               | Regex          | 1                         | 8    |
| UK Drivers License Numbers                           | Regex          | 50                        | 4    |
| UK Passport Number                                   | Regex          | 5                         | 1    |
| USBank Routing Numbers - California                  | Regex          | 50                        | 1    |
| United Bank Routing Number - California              | Regex          | 1                         | 1    |
| Wells Fargo Routing Numbers - California             | Regex          | 10                        | 1    |
| aws_access_key                                       | Regex          | 1                         | 3    |
| aws_credentials_context                              | Regex          | 1                         | 3    |
| aws_secret_key                                       | Regex          | 1                         | 10   |
| facebook_secret                                      | Regex          | 1                         | 8    |
| github_key                                           | Regex          | 1                         | 8    |
| google_two_factor_backup                             | Regex          | 1                         | 8    |
| heroku_key                                           | Regex          | 1                         | 7    |
| microsoft_office_365_oauth_context                   | Regex          | 1                         | 1    |
| pgSQL Connection Information                         | Regex          | 1                         | 2    |
| slack_api_key                                        | Regex          | 1                         | 7    |
| slack_api_token                                      | Regex          | 1                         | 8    |
| ssh_dss_public                                       | Regex          | 1                         | 1    |
| ssh_rsa_public                                       | Regex          | 1                         | 1    |

## Speed up the process
The recommendation that I will make is of course to improve the performances of the algorithm but this can be an hard task. The other solution will take a large amount of rows like 200k and test the Pii data on those rows. Of course if you need to comply against GDPR you need to read all rows of the dataframe. Even reading through all rows I don't guaranty the output of this algortim is 100% objective.

# Conclusion
Asserting that the data does not contains any PII data is not a simple task and it is expensive. I strongly recommend if you have the budget for that to use [AWS Macie](https://docs.aws.amazon.com/macie/latest/userguide/macie-classify-objects-regex.html). Or having strong DataScientist that can improve this algorithm.