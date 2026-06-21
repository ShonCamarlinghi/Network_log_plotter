
Network Data Log Cleaner and Plotter.

This technical note provides an overview, architecture breakdown, and operational guide for the plotter repository. The system automated the parsing, filtering, and graphical visualization of 24-hour network performance datasets.

1. System Overview
   The core purpose of this toolchain was to ingest raw network traffic or metric logs, strip away corrupted or unparseable entries,
   compute running averages, and output a time-series chart mapping network trends over a standard daily cycle.
   The software stack relies on a multi-language architecture:
   Python (97.6%): Orchestrates data flow, manages bad data routing, and handles chart generation.
   AWK: Used for lightweight, streaming statistical aggregations.

3. Component directory architecture.
    The workspace is organized into explicit functional domains to separate input runtime files from processing logic:
├── .idea/                # Local JetBrains IDE environment metadata
├── logDir/               # Input repository for raw 24-hour network text logs
├── my_env/               # Local Python virtual environment containing dependencies
├── resultDir/            # Output target for generated visual graphs (PNG/PDF)
├── BAD_DATA.txt          # Quarantine file isolated during data-cleaning runs
├── README.md             # High-level repository summary
├── avg.awk               # Stream processing script to compute data-point averages
└── log_plotter.py        # Master execution script handling pipeline logic & plotting

4. Data processing pipeline.
   Execution flows through three sequential steps.

   Stage 1: Data Ingestion and Sanitization
   Execution Engine: log_plotter.pyAction: The script reads raw raw log files from logDir/.
   Structural anomalies, null values, or incomplete lines are automatically stripped out and written to BAD_DATA.txt
   to keep the primary pipeline stable.

   Stage 2: Aggregation
   Execution Engine: avg.awk
   Action: The sanitized network metrics are passed through the AWK script. It processes timestamps and aggregates columns
   (such as throughput, latency, or packet volumes) into calculated averages.

   Stage 3: Visual Rendering
   Execution Engine: log_plotter.pyAction: Python reads the compiled averages, maps them across a 24-hour timeline,
   and outputs polished visual trends to resultDir/.

5. Dependencies and environment
  The script isolates its third-party requirements (expectedly packages like matplotlib or pandas) inside the my_env/ virtual directory.
  To recreate the runtime state, you must source this environment or review its binaries if you migrate the project to a modern system.   
