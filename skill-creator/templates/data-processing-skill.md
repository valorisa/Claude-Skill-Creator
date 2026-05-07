# Log Analyzer

Parse, filter, and summarize log files to identify errors, patterns, and anomalies.

## Trigger

Use when the user says "analyze these logs", "what went wrong", "parse this log file", `/log-analyze`, or provides a log file path to investigate.

## Process

1. Read the log file or piped input
2. Detect the log format automatically (JSON lines, syslog, Apache, custom delimited)
3. Extract and categorize entries:
   - Errors and exceptions (with stack traces grouped)
   - Warnings
   - Patterns (repeated messages, frequency spikes)
   - Timeline (first occurrence, last occurrence, duration)
4. Identify anomalies:
   - Sudden increase in error rate
   - New error types not seen in the first 20% of the file
   - Gaps in timestamps (service downtime indicators)
5. Present a summary report:
   - Top 5 most frequent errors with count
   - Timeline of incidents
   - Suggested root cause if patterns indicate one
6. Ask: "Want me to dig deeper into any specific error, or filter by time range?"

## Constraints

- Do not modify the original log file
- If the file is larger than 10,000 lines, sample intelligently (head + tail + error-dense sections)
- Do not guess root causes without pattern evidence — say "insufficient data" if unsure
- Preserve exact timestamps in output (do not convert timezones unless asked)

## Output

A structured summary report displayed to the user with error counts, timeline, and patterns. Optionally filtered views on request.
