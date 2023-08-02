# Adding GitHub Triggers

## visualize how Trigger works in Tekton

Here is a diagram that visualizes the interconnection between the EventListener, Trigger, TriggerBinding, and TriggerTemplate in Tekton:

![Diagram](https://showme.redstarplugin.com/d/s5CJsTeI)

- The EventListener triggers a Trigger when an event is received.
- The Trigger refers to a TriggerBinding and a TriggerTemplate.
- The TriggerBinding extracts data from the event payload and stores it as parameters.
- The TriggerTemplate uses these parameters and specifies the PipelineRun or TaskRun to be executed.

[You can view this diagram in a new tab.](https://showme.redstarplugin.com/d/s5CJsTeI)

[You can edit this diagram online if you want to make any changes.](https://showme.redstarplugin.com/s/c2mkRQ1X)

The type of the diagram is a graph in Mermaid language. 

To view ideas for improving the diagram, use the key phrase "*show ideas*"

To view other types of diagram and languages, use the key phrase "*explore diagrams*"

