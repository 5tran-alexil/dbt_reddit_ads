<p align="center">
    <a alt="License"
        href="https://github.com/fivetran/dbt_reddit_ads/blob/main/LICENSE">
        <img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" /></a>
    <a alt="dbt-core">
        <img src="https://img.shields.io/badge/dbt_Core™_version->=1.3.0_,<2.0.0-orange.svg" /></a>
    <a alt="Maintained?">
        <img src="https://img.shields.io/badge/Maintained%3F-yes-green.svg" /></a>
    <a alt="PRs">
        <img src="https://img.shields.io/badge/Contributions-welcome-blueviolet" /></a>
</p>

# Reddit Ads Transformation dbt Package ([Docs](https://fivetran.github.io/dbt_reddit_ads/))
# 📣 What does this dbt package do?
- Produces modeled tables that leverage Reddit Ads data from [Fivetran's connector](https://fivetran.com/docs/applications/x) in the format described by [this ERD](https://fivetran.com/docs/applications/x#schemainformation) and builds off the output of our [Reddit Ads source package](https://github.com/fivetran/dbt_reddit_ads_source).
- Enables you to better understand the performance of your ads across varying grains:
  - Providing an account, campaign, ad group, keyword, ad, utm and search level reports.
- Materializes output models designed to work simultaneously with our [multi-platform Ad Reporting package](https://github.com/fivetran/dbt_ad_reporting).
- Generates a comprehensive data dictionary of your source and modeled Reddit Ads data through the [dbt docs site](https://fivetran.github.io/dbt_reddit_ads/).

The following table provides a detailed list of all models materialized within this package by default. 
> TIP: See more details about these models in the package's [dbt docs site](https://fivetran.github.io/dbt_reddit_ads/#!/overview?g_v=1&g_e=seeds).

| **Model**                | **Description**                                                                                                                                |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| [reddit_ads__account_report](https://fivetran.github.io/dbt_reddit_ads/#!/model/model.reddit_ads.reddit_ads__account_report)             | Each record in this table represents the daily performance at the account level. |
| [reddit_ads__campaign_report](https://fivetran.github.io/dbt_reddit_ads/#!/model/model.reddit_ads.reddit_ads__campaign_report)            | Each record in this table represents the daily performance of a campaign at the campaign/advertising_channel/advertising_channel_subtype level. |
| [reddit_ads__ad_group_report](https://fivetran.github.io/dbt_reddit_ads/#!/model/model.reddit_ads.reddit_ads__ad_group_report)            | Each record in this table represents the daily performance at the ad group level. |
| [reddit_ads__ad_report](https://fivetran.github.io/dbt_reddit_ads/#!/model/model.reddit_ads.reddit_ads__ad_report)            | Each record in this table represents the daily performance at the ad level. |
| [reddit_ads__post_report](https://fivetran.github.io/dbt_reddit_ads/#!/model/model.reddit_ads.reddit_ads__post_report)            | Each record in this table represents the daily performance at the post level. |
| [reddit_ads__url_report](https://fivetran.github.io/dbt_reddit_ads/#!/model/model.reddit_ads.reddit_ads__url_report)            | Each record in this table represents the daily performance of URLs at the ad level. |

# 🎯 How do I use the dbt package?

## Step 1: Prerequisites
To use this dbt package, you must have the following:

- At least one Fivetran Reddit Ads connector syncing data into your destination.
- A **BigQuery**, **Snowflake**, **Redshift**, **PostgreSQL**, or **Databricks** destination.

### Databricks Dispatch Configuration
If you are using a Databricks destination with this package you will need to add the below (or a variation of the below) dispatch configuration within your `dbt_project.yml`. This is required in order for the package to accurately search for macros within the `dbt-labs/spark_utils` then the `dbt-labs/dbt_utils` packages respectively.
```yml
dispatch:
  - macro_namespace: dbt_utils
    search_order: ['spark_utils', 'dbt_utils']
```

## Step 2: Install the package
Include the following reddit_ads package version in your `packages.yml` file:
> TIP: Check [dbt Hub](https://hub.getdbt.com/) for the latest installation instructions or [read the dbt docs](https://docs.getdbt.com/docs/package-management) for more information on installing packages.
```yaml
packages:
  - package: fivetran/reddit_ads
    version: [">=0.1.0", "<0.2.0"]
```
## Step 3: Define database and schema variables
By default, this package runs using your destination and the `reddit_ads` schema. If this is not where your Reddit Ads data is (for example, if your Reddit Ads schema is named `reddit_ads_fivetran`), add the following configuration to your root `dbt_project.yml` file:

```yml
vars:
    reddit_ads_database: your_destination_name
    reddit_ads_schema: your_schema_name 
```

## (Optional) Step 4: Additional configurations
<details><summary>Expand for configurations</summary>

### Change the build schema
By default, this package builds the Reddit Ads staging models within a schema titled (`<target_schema>` + `_reddit_ads_source`) and your Reddit Ads modeling models within a schema titled (`<target_schema>` + `_reddit_ads`) in your destination. If this is not where you would like your Reddit Ads data to be written to, add the following configuration to your root `dbt_project.yml` file:

```yml
models:
    reddit_ads_source:
      +schema: my_new_schema_name # leave blank for just the target_schema
    reddit_ads:
      +schema: my_new_schema_name # leave blank for just the target_schema
```
    
### Change the source table references
If an individual source table has a different name than the package expects, add the table name as it appears in your destination to the respective variable:

> IMPORTANT: See this project's [`dbt_project.yml`](https://github.com/fivetran/dbt_reddit_ads/blob/main/dbt_project.yml) variable declarations to see the expected names.

```yml
vars:
    reddit_ads_<default_source_table_name>_identifier: your_table_name 
```

</details>

## (Optional) Step 5: Orchestrate your models with Fivetran Transformations for dbt Core™    
<details><summary>Expand for more details</summary>

Fivetran offers the ability for you to orchestrate your dbt project through [Fivetran Transformations for dbt Core™](https://fivetran.com/docs/transformations/dbt). Learn how to set up your project for orchestration through Fivetran in our [Transformations for dbt Core setup guides](https://fivetran.com/docs/transformations/dbt#setupguide).

</details>

# 🔍 Does this package have dependencies?
This dbt package is dependent on the following dbt packages. Please be aware that these dependencies are installed by default within this package. For more information on the following packages, refer to the [dbt hub](https://hub.getdbt.com/) site.
> IMPORTANT: If you have any of these dependent packages in your own `packages.yml` file, we highly recommend that you remove them from your root `packages.yml` to avoid package version conflicts.
    
```yml
packages:
    - package: fivetran/reddit_ads_source
      version: [">=0.1.0", "<0.2.0"]

    - package: fivetran/fivetran_utils
      version: [">=0.4.0", "<0.5.0"]

    - package: dbt-labs/dbt_utils
      version: [">=1.0.0", "<2.0.0"]

    - package: dbt-labs/spark_utils
      version: [">=0.3.0", "<0.4.0"]
```
# 🙌 How is this package maintained and can I contribute?
## Package Maintenance
The Fivetran team maintaining this package _only_ maintains the latest version of the package. We highly recommend you stay consistent with the [latest version](https://hub.getdbt.com/fivetran/reddit_ads/latest/) of the package and refer to the [CHANGELOG](https://github.com/fivetran/dbt_reddit_ads/blob/main/CHANGELOG.md) and release notes for more information on changes across versions.

## Opinionated Decisions
In creating this package, which is meant for a wide range of use cases, we had to take opinionated stances on a few different questions we came across during development. We've consolidated significant choices we made in the [DECISIONLOG.md](https://github.com/fivetran/dbt_reddit_ads/blob/main/DECISIONLOG.md), and will continue to update as the package evolves. We are always open to and encourage feedback on these choices, and the package in general.

## Contributions
A small team of analytics engineers at Fivetran develops these dbt packages. However, the packages are made better by community contributions! 

We highly encourage and welcome contributions to this package. Check out [this dbt Discourse article](https://discourse.getdbt.com/t/contributing-to-a-dbt-package/657) on the best workflow for contributing to a package!

# 🏪 Are there any resources available?
- If you have questions or want to reach out for help, please refer to the [GitHub Issue](https://github.com/fivetran/dbt_reddit_ads/issues/new/choose) section to find the right avenue of support for you.
- If you would like to provide feedback to the dbt package team at Fivetran or would like to request a new dbt package, fill out our [Feedback Form](https://www.surveymonkey.com/r/DQ7K7WW).
- Have questions or want to just say hi? Book a time during our office hours [on Calendly](https://calendly.com/fivetran-solutions-team/fivetran-solutions-team-office-hours) or email us at solutions@fivetran.com.
