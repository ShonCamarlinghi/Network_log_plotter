
<div align="center">

<img width="80%" src="resultDir/traceroute_perf.sh.8266.run_out/traceroute_perf.sh.8266.run_plot.png"/>

<h1>Network Data Log Cleaner and Plotter</h1>
<h3>Don't just grep your network logfiles, plot them!</h3>
</div>
<p>This technical note provides an overview, architecture breakdown, and operational guide for the plotter repository. The system automated the parsing, filtering, and graphical visualization of 24-hour network performance datasets.</a>.</p>

1. System Overview.\
   The core purpose of this toolchain was to ingest raw network traffic or metric logs, strip away corrupted or unparseable entries,
   compute running averages, and output a time-series chart mapping network trends over a standard daily cycle.
 
   The software stack relies on a multi-language architecture: \
   Python (97.6%) | Orchestrates data flow, manages bad data routing, and handles chart generation. \
   AWK | Used for lightweight, streaming statistical aggregations.

2. Component directory architecture.\
   The workspace is organized into explicit functional domains to separate input, output files from processing logic:
<img width="674" height="181" alt="Image" src="https://github.com/user-attachments/assets/6e11a671-ad27-4200-a59a-fcfa07a381a3" />

3. Data processing pipeline. \
   Execution flows through three sequential steps.

   Stage 1: Data Ingestion and Sanitization | log_plotter.py \
   The script reads raw raw log files from logDir/.
   Structural anomalies, null values, or incomplete lines are automatically stripped out and written to BAD_DATA.txt
   to keep the primary pipeline stable.

   Stage 2: Aggregation | avg.awk \
   The sanitized network metrics are passed through the AWK script. It processes timestamps and aggregates columns \
   (such as throughput, latency, or packet volumes) into calculated averages. \
   This humble awk script performs Statistical Feature Engineering (Temporal Abstraction), where raw log data \
   is transformed into structured numerical vectors representing traffic characteristics (such as throughput, latency, or packet volumes).
 
   Stage 3: Visual Rendering | log_plotter.pyAction \
   Python reads the compiled averages, maps them across a 24-hour timeline,
   and outputs polished visual trends to resultDir/.

5. Dependencies and environment \
  The script isolates its third-party requirements (expectedly packages like matplotlib or pandas) inside the my_env/ virtual directory.
  To recreate the runtime state, you must source this environment or review its binaries if you migrate the project to a modern system.   


