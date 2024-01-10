---
title: "Spreadsheet with Database Connection Comparison"
editor: visual
date: '2023-12-11'
description: Compares different spreadsheet apps and how well they support direct database reading and writing, and any restrictions
tags: 
- Google Sheets
- numbers
- excel
- LibreOffice Calc
- spreadsheet
- database
- comparison
---

December 11, 2023

### Overview

This document contains information on what is needed to connect to databases from various Spreadsheets, to read and write data, and lists any restrictions where they exist

In general, it's much easier to get data put directly into a spreadsheet from a database than to update a database from a spreadsheet. In all cases, using CSV files for reading and/o writing is a valid option

The use of CSV files instead of directly reading/writing to the database is supported by all spreadsheets

<style>
    .compare_table th {
        background-color: lightgrey;
    }
</style>

<div class="compare_table">
| Spreadsheet | Reading DB | Writing DB | Notes |
| --- | :---: | :---: | --- | --- |
| [Google Sheets](#google-sheets) <span style="color:darkgreen">✔</span> | <span style="color:black">✔✔✔</span> | <span style="color:black">✔✔</span> | Write scripts to move data; programming skills help |
| [Numbers](#numbers) <span style="color:red">✖</span> | <span style="color:darkgrey">➖</span> | <span style="color:darkgrey">➖</span> |   |
| [Excel (Windows)](#excel-windows) <span style="color:darkgreen">✔</span> | <span style="color:black">✔✔✔</span> | <span style="color:black">✔</span> | Update DB with CSV is easiest; need ODBC driver |
| [Excel (Mac)](#excel-mac) <span style="color:darkgreen">✔</span> |<span style="color:black"> ✔</span> | <span style="color:black">✔</span> | SQLServer is supported; buy other ODBC drivers |
| [LibreOffice Calc](#libre-office-calc) <span style="color:grey">≈</span> | <span style="color:black">✔✔</span> | <span style="color:darkgrey">➖</span> |   |   |
</div>
_Spreadsheet Rating:_ _✖_ _Not supported_ _✔_ _Supported_ _≈_ _Mixed support_ \
_Reading/Writing Difficulty: ✔ Hard ✔✔ Medium ✔✔✔ Easy ➖Not supported_

### How Tested

A sample MySQL database was started on the OpenStack instance at the University of Arizona, and made publicly accessible (password protected)

LibreOffice Calc was tested on an Ubuntu 22.04 system

Different database instances may have different requirements. For example, needing to be on a particular VPN, or using two-factor authentication

### Google Sheets

Scripts are written to read and write data from a database using Google provided connectors

**Restrictions**

Postgres and its derivatives (PostGIS, et. al.) are not supported

**Reading**

A generic [google](https://developers.google.com/apps-script/overview#:~:text=Google%20Apps%20Script%20is%20a,Calendar%2C%20Drive%2C%20and%20more.)[script](https://developers.google.com/apps-script/overview#:~:text=Google%20Apps%20Script%20is%20a,Calendar%2C%20Drive%2C%20and%20more.) can be used to pull data into a spreadsheet and examples are easy to find. For example, [https://www.lido.app/tutorials/google-sheets-connect-to-database](https://www.lido.app/tutorials/google-sheets-connect-to-database). Adjust the SQL statement to meet your needs and be sure to specify what the sheet name is in the script(s)

**Writing**

Writing to the database can be easy if all the data in a spreadsheet is to be sent to a table in the database. Google documentation provides a simple way of doing that: [https://developers.google.com/apps-script/guides/jdbc](https://developers.google.com/apps-script/guides/jdbc). If data filtering is needed, or if data is from multiple sheets, then it's necessary to accumulate each row of data to insert into the database before writing it. This can become complicated, so it might be easier to have a special sheet with the data to push to the database and use a generic script (with necessary modifications, such as table and column names)

**Conclusion**

Most flexible solution for directly reading and writing to a database. However, some programming skills come in handy, and having a script separate from the spreadsheet seems a little weird

### Numbers

This Apple spreadsheet application doesn't connect to databases. Use other means to use CSV files for fetching data fo the spreadsheet and updating the database

**Conclusion**

No database connections supported

### Excel (Windows)

Setting up database connectivity can be painful and complicated, and may require a system administrator.

**Reading**

Connect to the database using the ODBC connector that matches the database you are pulling data from. You can select tables and columns, or write SQL commands to fetch the data into your spreadsheet. Microsoft has a [web page](https://support.microsoft.com/en-us/office/import-data-from-data-sources-power-query-be4330b3-5356-486c-a168-b68e9e616f5a?redirectSourcePath=%252farticle%252f8760c647-88b9-409d-b312-6ea8f84a269b) containing instructions for multiple data sources; scroll down to see the different sources and their instructions.

The following download links may help:

- Visual Studio redistributables: [https://my.visualstudio.com/Downloads](https://my.visualstudio.com/Downloads)
- ODBC drivers: https://dev.mysql.com/downloads/connector/odbc/

**Writing**

It's much easier to upload a CSV file into the database than to use other methods. Depending upon the database, a search online can provide you with the means for directly writing to the database (or as directly as possible)

For example, MySQL methods for writing to the database can be found at [https://nanonets.com/blog/import-excel-into-mysql/](https://nanonets.com/blog/import-excel-into-mysql/)

**Conclusion**

While it can be a pain getting the needed software onto the system, you only need to do it once for each database type. Excel provides a direct read/write solution to databases

### Excel (Mac)

**Restrictions**

Only SQLServer is supported "out of the box". For other databases it's necessary to pay for an ODBC driver to connect

**Reading**

Use the SQLServer connectors to read from the database, if that's an option. Otherwise, generate a CSV file with the data wanted and load that into the spreadsheet

**Writing**

Use the SQLServer connectors to write to the database, if that's an option. Otherwise, generate a CSV file with the generated data and upload that into the database

**Conclusion**

If you aren't connecting to SQLServer, you need to use CSV files

### Libre Office Calc

Install the libreoffice-base package to add the _Database_ option to Calc; this may require an administrator to perform and a reboot after it's installed. Install the appropriate JDBC driver somewhere accessible on the system (you will navigate to the driver when establishing a connection in the next step)

The following instructions are written for the MySQL database, but can be easily adjusted for other databases: https://techsparx.com/blog/2016/08/libreoffice-mysql.html

**Reading**

Perform a SQL Query to import the data into a spreadsheet

**Writing**

Unable to find a documented way to put data back into a modern relational database. Generate a CSV file and update the database with that

**Conclusion**

Easier to set up on Linux-based systems than Excel on Windows is (see above for Excel on Mac). Pulling data directly is on par with other spreadsheet applications, but direct writing appears to be missing.