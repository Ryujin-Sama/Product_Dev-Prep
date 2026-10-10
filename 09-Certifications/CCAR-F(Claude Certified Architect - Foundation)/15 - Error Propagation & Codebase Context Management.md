
### Vague Errors Paralyze Coordinators

Generic errors are like a contractor saying things didn't work out. The coordinator needs a detailed decision brief to recover the pipeline.

### Building a Recovery-Ready Error Response

#### Error Category:
What kind of failure was this? This helps to drives the recovery strategy

#### is Retryable:
The single most important boolean. True = retry and false = structural problem.

#### Attempted Query:
What was the subagent trying to do? This identifies the research gap.

#### Partial Results + alternatives
What data was saved and what else can we try? This enables graceful degradation

