# Azure Data Factory – REST API Data Ingestion Using Pagination & Offset

## 📌 Project Overview

This project demonstrates how to ingest data from a **REST API using Azure Data Factory (ADF)** when the API returns data across multiple pages.

The pipeline uses **offset-based pagination**, where the offset increases by **20** for each subsequent API request.

### Technologies Used

* Azure Data Factory
* REST API
* Copy Activity
* Pagination
* Offset
* Parameterized URLs

---

# 🏗️ Pipeline Architecture

```text
REST API
   ↓
Azure Data Factory
   ↓
Copy Activity
   ↓
Pagination / Offset
   ↓
Destination
```

The **Copy Activity** connects to the REST API, retrieves the data page by page, and loads the complete dataset into the destination.

---

# 🔄 How Pagination Works

The API does not return all the data in a single request. Instead, the data is divided into multiple pages.

This project uses **offset-based pagination**, where the offset increases by **20** for each subsequent request.

### Example

```text
Page 1 → Offset = 0  → Records 0–20
Page 2 → Offset = 20 → Records 21–40
Page 3 → Offset = 40 → Records 41–60
Page 4 → Offset = 60 → Records 61–80
...
```

The offset tells the API where to start retrieving the next set of data.

ADF dynamically handles the offset and continues requesting data until all available records have been retrieved.

> **Note:** The exact number of records returned per request depends on how the API implements its offset logic.

---

# ⚙️ API Configuration

The REST API is configured using a **Base URL** and **Relative URL**.

### Base URL

```text
https://api.example.com
```

### Relative URL

```text
/data?offset={offset}
```

The `{offset}` parameter is dynamically replaced during each API request.

For example:

```text
/data?offset=0
/data?offset=20
/data?offset=40
/data?offset=60
```

This allows the pipeline to retrieve multiple pages without manually creating separate API requests.

---

# 🔹 Pagination Configuration in ADF

Pagination is configured inside the **Copy Activity → Source → Pagination Rules** section.

The pagination rule tells ADF how to retrieve subsequent pages from the API.

In this project, the API uses an **offset parameter** to identify the next set of records.

### Pagination Flow

```text
Initial Request
      ↓
Offset = 0
      ↓
Retrieve Data
      ↓
Offset = 20
      ↓
Retrieve Next Data
      ↓
Offset = 40
      ↓
Retrieve Next Data
      ↓
Continue Until All Data Is Retrieved
```

---

# 🔢 Offset-Based Example

Suppose the API contains 100 records.

The pipeline requests the data using the following offsets:

| Page | Offset | Data Range |
| ---- | -----: | ---------- |
| 1    |      0 | 0–20       |
| 2    |     20 | 21–40      |
| 3    |     40 | 41–60      |
| 4    |     60 | 61–80      |
| 5    |     80 | 81–100     |

The process continues until the complete dataset has been retrieved.

---

# 🔗 Dynamic Relative URL

A dynamic relative URL is used so that the same pipeline can handle multiple API requests.

```text
/data?offset={offset}
```

Here:

* `data` → API endpoint
* `offset` → pagination parameter
* `{offset}` → dynamic value that changes for each request

For example:

```text
Offset = 0
→ /data?offset=0

Offset = 20
→ /data?offset=20

Offset = 40
→ /data?offset=40
```

This makes the pipeline more **dynamic and reusable**.

---

# 📸 Pipeline

Add the screenshot of your complete ADF pipeline:

```text
![ADF Pipeline](Screenshots/Pipeline.png)
```

---

# 📸 REST API Source Configuration

Add the screenshot showing the REST API source configuration:

```text
![REST API Source](Screenshots/Source_Configuration.png)
```

---

# 📸 Pagination Configuration

Add the screenshot showing the pagination rule:

```text
![Pagination Configuration](Screenshots/Pagination.png)
```

---

# 📸 Output

Add the screenshot showing the successfully retrieved data:

```text
![Output](Screenshots/Output.png)
```

---

# 🎯 Key Learnings

Through this project, I gained practical experience in:

* Connecting Azure Data Factory with REST APIs
* Using **Copy Activity** for API data ingestion
* Handling API pagination
* Implementing **offset-based pagination**
* Creating dynamic API URLs
* Using parameters for API requests
* Retrieving complete datasets from paginated APIs
* Loading API data into a destination

---

# 🚀 Future Improvements

Possible improvements for this pipeline include:

* Implementing incremental API ingestion
* Adding error handling and retry mechanisms
* Adding logging and monitoring
* Parameterizing the API endpoint
* Creating a metadata-driven API ingestion framework
* Integrating the pipeline with Azure Data Lake Storage
* Processing the ingested data using Databricks/Spark

---

# 👨‍💻 Author

**Arif Ali**

Business Intelligence Analyst | Qlik Consultant | Aspiring Data Engineer

This project is part of my journey toward building practical **Data Engineering solutions using Azure and Big Data technologies**.
