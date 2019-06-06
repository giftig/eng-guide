# Logging

## Log content
Log content should be considered carefully; remember this is what will help you see your
application is working and help you debug issues, including in live at 3am. Logs must be:

**Easy to read**:
- Logs should be written in plain english in a format which is easily read by humans, not by
  machines. Anything intended to be read by a machine should use a different mechanism, such as
  cloudwatch metrics, an API call to a monitoring tool, etc.
- Log lines should not be excessively long and should never span multiple lines, except in the
  case of reporting error stack traces. Very long lines are much harder to read, and multiple-line
  log statements make it very difficult to distinguish between one and the other. Logging large
  JSON objects, for example, disrupts the log very badly, even at DEBUG level.
- At the appropriate level:
    - Internal application errors and major faults which are either unrecoverable or required some
      sort of recovery mechanism to be enacted (retries, dead letter writing, etc.) should be
      reported as `ERR`: *Failed to connect to postgres! Could not write record 123456!*
    - Validation errors, data input errors, client errors, and the like should be reported as
      `WAR`: *e-mail "fakeymcfakefake@fake" is not a valid e-mail address; ignoring record*
    - Regular high-level activity should be reported at `INF`; this should usually be an activity
      which is a recognisable function of the application and doesn't require in-depth knowledge of
      the application's internals to understand: *Writing 290383 bytes into /tmp/foo/output.json*
    - Detailed internal should be reported at `DEB`: *Calculating filename for record 123456*
- With an appropriate pattern:
  - This is subjective but in logback, for example, a pattern like
    `%d{yyyy-MM-dd HH:mm:ss.SSS,UTC} %-3level %logger{26}: %msg %ex{full}%n` is a typical choice.
    Try to balance length of the prefix with having sufficient information to understand the
    context of the logging.
