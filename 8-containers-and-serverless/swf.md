# SWF - Simple Workflow Service

- Allows us to build workflows used to coordinate activities over distributed components (example: order flow)
- Allows to build complex, automated and human involved workflows
- It is the predecessor of Step Functions, it uses instances/servers
- It allows the usage of the same patterns/anti patterns like long running workflows
- AWS recommends defaulting to Step Functions instead of SWF, use SWF only if there is very specific workflow that requires it
- Workflow: set of activities that carry out some objective together with the logic that coordinates the activities
- Within workflows we have activity task and activity workers
- A worker is program that we create to perform tasks
- Workflows have deciders, applications that run on an unit of compute of AWS
- Deciders schedule activity tasks, provides input data to activity workers, processes events and ends the workflow when the object is completed
- SWF workflows can run for max 1 year

## SWF vs Step Functions

- Default: Step Functions - they are serverless, they require lower admin overhead
- AWS FLow Framework - way of defining workflows supported by SWF
- External Signals to intervene in process, we need SWF
- Launch child flows and have the processing return to parent, we need ot use SWF
- Bespoke/complex decision logic: use SWF (custom decider application)
- Mechanical Turk integration: use SWF (suggested AWS architecture)