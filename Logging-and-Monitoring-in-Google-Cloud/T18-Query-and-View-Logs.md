# 🔍 Query and View Logs

Once logs are collected and routed to the appropriate destination, you can **query and view logs** using **Logs Explorer**.

---

## 🖥️ Logs Explorer Interface

The interface consists of several panes:

- **Action Toolbar:**  
  Refine logs to projects or storage views, share links, and access Logs Explorer help.

- **Query Pane:**  
  Build queries, view recently used or saved queries.

- **Results Toolbar:**  
  Show/hide logs, toggle histogram pane, create log-based metrics or alerts.

- **Jump to Now:**  
  Quickly query logs for the current time.

- **Query Results:**  
  Detailed results with summary and timestamps for troubleshooting.

- **Log Fields Pane:**  
  Filter logs by resource type, log name, project ID, and more.

- **Histogram Panel:**  
  Visualize query results over time. Each bar represents a time range, color-coded by **severity**.

---

## 🧩 Building Queries

Queries select entries displayed in Logs Explorer. Methods include:

- **Logging Query Language (LQL)**
- Drop-down menus
- Logs Field Explorer
- Clicking fields in query results

**Tips to start:**

1. Start with known information: resource, log file, severity.
2. Use the **Query Builder** drop-down menus for filtering:
   - **Resource:** `resource.type` (AND logic for multiple entries)
   - **Log Name:** `logName` (OR logic for multiple entries)
   - **Severity:** `severity` (OR logic for multiple entries)

---

## ⚡ Advanced Query Operators

- **Comparison Operators:** `=`, `!=`, `<`, `>`, `<=`, `>=`
- **Colon (`:`) Operator:** Check if a value exists or substring matches
- **Wildcard (`:*`) Operator:** Check if a field exists without testing a specific value

---

## ⏱️ Time Ranges

- Narrow logs by **pre-defined ranges**, **custom range**, or **jump to a specific time**.
- Histogram panel syncs with the selected time range to show log distribution.

---

## 🧮 Log Field Panel

- Provides high-level summary of log data.
- Shows **counts of log entries** sorted by frequency.
- Counts incrementally load as the query runs.
- Fields can be added to the **Query Builder** to refine queries.

---

## 📊 Histogram Panel

- Visualizes log distribution over time.
- **Severity colors** help spot trends and errors.
- Click a bar → **Jump to Time** → Drill into specific intervals.

---

## 🔗 Boolean Expressions

Advanced queries support **AND, OR, NOT**:

- Use **all caps** for operators.
- **Precedence:** NOT > OR > AND
- **AND & OR:** Short-circuit operators

---

## 📝 Finding Log Entries

**Recipe for searching:**

1. Start with known info: log filename, resource name, or message content.
2. Full-text search can be slow but effective.
   - Example: `/score called`
   - Restrict searches to a field: `jsonPayload:"/score called"` or `jsonPayload.message="/score called"`

**SEARCH Function:**
```text
SEARCH([query])
SEARCH([field], [query])
```
- First form searches the entire log entry.
- Second form searches a specific field.
- Efficient for string matching, not for non-text fields.

## 💡 Tips for Efficient Log Queries

- Use indexed fields: log entry name, resource type, resource labels.
- Apply constraints on resource and labels:
```text
resource.type = "gke_cluster"
resource.labels.namespace = "my-cool-namespace"
```
- Be specific with log names.
- Limit time ranges to reduce queried data.
