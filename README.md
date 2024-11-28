<!-- badges: start -->

[![Documentation Status](https://readthedocs.org/projects/nvi-documentation/badge/?version=latest)](https://nvi-documentation.readthedocs.io/en/latest/?badge=latest)

<!-- badges: end -->

# NVI Documentation

The Read the Docs version is here:

https://nvi-documentation.readthedocs.io/en/latest/index.html

# Updating the documentation

The **master** branch now is renamed to **main**. And it is now protected. 

Please do not fork the repository when you want to make changes. 

Instead, checkout the **dev** branch and make changes on **dev**. Ideally you should make a new branch from **dev** on the same repository, and then PR to merge to **dev**.

Ping your teammate for review before merging **dev**, ping Novica or Karin for merging **dev** to **main**.

Read the Docs is now set up to check PRs for successful builds. Then we only merge with **main** when those builds pass.

Follow the conventions on naming new files: `number-name_of_doc.md`. Update the appropriate `index.rst` to have the file appear in the TOC.
