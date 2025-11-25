# 📝 Audit Logs Entry Format

Once you have enabled the necessary audit logs, it's important to understand the **structure of audit log entries**.

---

## 📦 LogEntry Object

- Every audit log entry in Cloud Logging is an **object of type `LogEntry`**.
- What distinguishes **audit log entries** from other logs is the **`protoPayload`** field:
  - Contains an **`AuditLog` object** that stores the audit logging data.

---

## 🔍 Key Fields in Audit Logs

1. **logName**
   - Identifies the type of log.
   - Example: Data Access audit logs.

2. **principalEmail**
   - Identifies the **principal generating the log entry**.

3. **operation**
   - Only exists for **large or long-running audit log entries**.

4. **serviceData**
   - Contains details about the action performed.
   - Example: In BigQuery, you can see the **actual query executed**.

> Tip: Use the [official Google Cloud service names list](https://cloud.google.com/logging/docs/audit#service_names) as a handy reference.

---

## 💡 Example Use Case

- If someone in your organization runs an **unexpected high-cost BigQuery query**:
  - You can check the **`principalEmail`** to see who ran it.
  - You can inspect **`serviceData`** to see what query was executed.
