# 🔥 Cloud Profiler

## 📌 What is Cloud Profiler?
- Cloud Profiler is a **continuous, low-overhead profiler** for production systems.
- It collects **CPU usage** and **memory allocation** data from live applications.
- Attributes resource consumption to **specific lines of source code**.

## ⭐ Why Use Cloud Profiler?
### 🟦 1. Low-Impact Production Profiling
- Profiling in dev environments often doesn't reflect real production load.
- Cloud Profiler runs with **minimal overhead** and **does not slow down** applications.

### 🟧 2. Detect Inefficient Code Automatically
- Continuously analyzes CPU- or memory-intensive functions.
- Shows an **interactive flame graph** of call hierarchy and resource usage.
- Helps identify **performance bottlenecks** and costly code paths.

### 🟩 3. Broad Language & Platform Support
- Works on applications running on Google Cloud or elsewhere.
- Supported languages: **Java, Go, Python, Node.js**.
- Profiling types vary per language — check documentation.
- ❗ Windows guest OS is **not supported**.

## 🧠 Key Profiling Metrics
### 🔵 CPU Profiles
- **CPU time**: time spent actively executing code.
- **Wall time**: total time including wait/sync blocks (always ≥ CPU time).

### 🟣 Heap Profiles
- **Heap**: memory allocated at capture time.
- **Allocated heap**: total allocations including freed memory.

### 🟠 Thread Profiles
- **Contention**: time threads spend waiting on others.
- **Threads**: thread counts.

## ⚙️ Setup Overview (Example: Python on App Engine)
1. Enable the **Cloud Profiler API**.
2. Install tools: C/C++ compiler, pip, and `googlecloudprofiler`.
3. Import the Profiler package.
4. Start the Profiler **early in the code**.
5. Optional: set **verbose logging** level (e.g., `3` for full debug output).

## 📊 Using the Profiler Interface
You can filter profiles by:
- Timespan  
- Service  
- Zone  
- Version  
- Profile type (CPU, heap, threads)

### 🔍 Weight Filter
- Restrict flame graph to **peak consumption** (e.g., top 5%).

### 🔄 Compare Profiles
- Compare two profiles for changes in performance.

## 🔥 Flame Graphs (How to Read Them)
- Visualize resource consumption across call stacks.
- Bottom = parent functions  
- Top = deeper calls  
- Wider blocks = higher resource usage  
- Collapsed and compact display for readability.

### Example Insight
- A function like `bar()` may consume most CPU time across multiple call paths.
- Optimizing such a function can significantly improve overall performance.

## 🖱️ Interactive Features
- Hover: shows function name, file, and specific metric usage.
- Click: zooms into selected call stack for deeper analysis.
