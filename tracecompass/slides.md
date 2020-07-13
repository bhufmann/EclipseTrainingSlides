---
title: Eclipse Trace Compass
theme: 'white'

revealOptions:
    transition: 'slide'

---
# Eclipse Trace Compass
* author: Bernd Hufmann
* author: Marc-André Laperle
* author: Patrick Tasse

---
# Agenda

- Day 1
    - Module 1: Overview and Background
    - Module 2: Core Trace API
	- Module 3: Analysis Framework
	- Module 4: Generic State System
- Day 2
    - Module 5: Time Graph Views
    - Module 6: Timing Analysis
	- Module 7: Timing Analysis Views
	- Module 8: Custom Analysis

---
# Approach to course

- A mix of theory and hands-on exercises
- Teams of 2
- Ask questions often and give feeback
- 9h30 to 16h30 schedule
- One hour lunch
- 15 minute break, morning and afternoon
- Please tell us if you are confused during the course

<center>**Let's get started!**</center>

---
# Module 1
## Overview and Background

---
# Trace Compass Overview

- **Framework** to build trace visualization and analysis tools

- **Scalable**: handle traces exceeding memory

- **Extensible** for any trace or log format. Binary, text, XML etc.

- **Reusable** views and widgets

- Available as standalone application or set of plug-ins


<center><img src="images/tracecompass.png"/></center>

---
# Trace Compass Overview

- **Open source** Eclipse Project
- **EPL**
- Mostly written in **Java**

---
# Trace Compass Overview
<center>
<div style="display:table-cell; width:70%;text-align: left;">
<ul>
<li>Quite active, growing community:
<ul>
	<li>Academia: École Polytechnique Montréal, Concordia, others</li>
	<li>Industry: Ericsson, Ciena, EfficiOS, others</li>
	<li>Government: NSERC, Prompt, DRDC, NOAA (US)</li>
	<li>Kalray</li>
	<li>Windriver</li>
</ul>
</li>
</ul>
</div>
<div style="display:table-cell; width:30%; text-align: center; vertical-align: middle"><img style="width:300px; height:auto" src="images/tracecompass_contributors.png"/></div>
<br/>
</center>
---
# Trace Compass Overview

<center><img src="images/tracecompass_overview.png"/></center>

---
## Common Features

- Management of traces, trace formats and experiments

<center><img src="images/tracecompass_trace_management.png"/></center>

---
## Common Features

- Package export and import

<center><img src="images/tracecompass_trace_package.png"/></center>

---
# Trace Compass Overview

- What is a trace in Trace Compass?
	- A Series of **events** over time
	- Each event has content **fields** (payload)

<center><img src="images/trace_events.png"/></center>

---
# Trace Compass Overview
## Common Features

- Events Table
	- Implemented as an Eclipse "editor"
	- The 'lifetime' of the trace instance is tied to the editor.

<center><img src="images/tracecompass_events_editor.png"/></center>

---
## Common Features

<center>
<p>
<div style="display:table-cell; width:50%;"><img style="width:300px; height:auto" src="images/tracecompass_searching.png"/></div>
<div style="display:table-cell; width:50%; text-align: center; vertical-align: middle"><span>**Searching**</span></div>
</p>
<p>
<div style="display:table-cell; width:50%;"><img style="width:300px; height:auto" src="images/tracecompass_filtering.png"/></div>
<div style="display:table-cell; width:50%; text-align: center; vertical-align: middle"><span>**Filtering**</span></div>
</p>
<p>
<div style="display:table-cell; width:50%;"><img style="width:300px; height:auto" src="images/tracecompass_highlighting.png"/></div>
<div style="display:table-cell; width:50%; text-align: center; vertical-align: middle"><span>**Highlighting**</span></div>
</p>
</center>
---
## Common Features

- Bookmarks and markers

<center><img src="images/tracecompass_bookmarks.png"/></center>

---
## Common Features

- Sequence Diagrams
	- Translates events to sequence diagram transaction
	- Extensions can define their own model

<center><img src="images/tracecompass_sequencediagrams.png"/></center>

---
# Trace Compass Overview
## State System

<center><img src="images/tracecompass_statesystem.png"/></center>

- State system abstracts events, analyses traces and creates models to be displayed

<center><img src="images/tracecompass_statesystem_events.png"/></center>

---
# Trace Compass Overview
## Control Flow View

- Displays processes state changes (color-coded) over time
	- USERMODE, SYSCALL, INTERRUPTED, WAIT_FOR_CPU, etc.

<center><img src="images/tracecompass_controlflowview.png"/></center>

---
# Trace Compass Overview
## Resources View

- Displays system resource states (color-coded) over time

<center><img src="images/tracecompass_resourcesview.png"/></center>

---
# Trace Compass Overview
## CPU Usage View

- Displays % of CPU used per thread over time

<center><img src="images/tracecompass_cpuusageview.png"/></center>

---
# Trace Compass Overview
## Call Stack View

- Shows the stack trace at any point during execution
- The stack in the tree on the left corresponds to the selected time in the time graph on the right

<center><img src="images/tracecompass_callstackview.png"/></center>

---
# Trace Compass Overview
## Critical Path

<center><img src="images/tracecompass_criticalpathview.png"/></center>

---
# Trace Compass Overview
## Timing Analysis

<center><img style="width:700px;" src="images/tracecompass_latencyviews.png"/></center>

---
# Trace Compass Overview
## Custom Text and XML Parsers

<center>
<div style="display:table-cell; width:50%; vertical-align: middle"><img style="width:400px;" src="images/tracecompass_customtext.png"/></div>
</center>



- Line based parser with regex defined in a wizard

- XML-based extracting data from XML elements and their attributes

---
# Trace Compass Overview
## Trace correlation (Experiments)

- Trace Compass can open **multiple traces** together to view it **as one**
	- This is called an **Experiment**
- Useful for
	- Traces coming from multiple nodes
	- Different languages
	- Different layers (network, etc.)
- Traces can be manually synchronized by time or use an automatic algorithm (extensible)

---
# Trace Compass Overview
## Integrations

- LTTng (UST, Kernel)
- Text Logs (custom parsers)
- Common Trace Format (Application, kernel, HW, Bare metal)
- Packet Capture
- BTF (Best Trace Format)
- GDB Trace Points

---
# Common Trace Format (CTF)

- CTF is one of the trace formats understood by Trace Compass
- A **metadata** file describes the trace structure, event fields, environment, etc.
	- CTF does not describe that structure, only the language to express it.
- **Channel** files contain binary data, separate from the **metadata**. There is where the events are stored.
- **LTTng** is a well-known tracer that generates CTF traces for **Kernel** and **User Space (UST)** domains.
- Trace Compass **reads** CTF traces directly using a Java-based parser.
- Trace Compass **does not** produce CTF traces, tracers like LTTng do.

---
# Trace Compass Overview
## References

- Project pages
	- <a href="http://tracecompass.org">tracecompass.org</a>
	- <a href="https://dev.eclipse.org/mailman/listinfo/tracecompass-dev">tracecompass-dev mailing List</a>
	- <a href="http://istmffastyet.dorsal.polymtl.ca">Is Trace Compass Fast Yet?</a>
	- <a href="http://lttng.org/">LTTng</a>
	- <a href="http://www.diamon.org/">Diamon Working Group</a>
	- <a href="http://tracingsummit.org/">Tracing Summit</a>

- Documentation
	- <a href="http://archive.eclipse.org/tracecompass/doc/org.eclipse.tracecompass.doc.user/User-Guide.html">Trace Compass User Guide</a>
	- <a href="http://archive.eclipse.org/tracecompass/doc/org.eclipse.tracecompass.doc.dev/Developer-Guide.html">Trace Compass Developer Guide</a>

---
# The example

- 2 processes exchanging messages (over sockets)
	- **master** process
	- **challenger** process
- **challenger** sends requests to **master** which replies
- Upon reception of a request **master** executes a **processing task**
- The processing task spawns multiple workers (simulated)
- Each worker performs some calculation that vary in **duration**
- Each calculation has processing states
- We trace **master**

---
# The example
<center><image src="images/tracecompass_example-explained.png"/></center>

---
# Module 2
## Core Trace API

---
# Signals

- Classes can register themselves to receive various Trace-related signals, this is built-in some base classes: `TmfComponent`, `TmfView`, etc.
- Uses Java annotation, `@TmfSignalHandler`, to mark method that receives the signal
- Some signals: `TmfTraceOpenedSignal`, `TmfTraceClosedSignal`, `TmfTraceRangeUpdatedSignal`

~~~java
public void foo() {
    TmfSignalManager.register(this);
}

@TmfSignalHandler
public void traceClosed(TmfTraceClosedSignal signal) {
    ...
}
~~~

---
# Steps to prepare

- In the command-line, cd to **~/workspace-training/EclipseTraining**. 
	- Execute **git reset --hard 3d96724**
- Execute the start script *~/workspace-training/start.tcsh*
- If you have any projects that are *not* named **External Plug-in Libraries**, remove them:
	- Right-click on the project, Delete (Do not check "Delete project contents on disk").
- You should only have *External Plug-in Libraries*
- All good?

---
# Steps to prepare

- Go to **Plug-in Development** perspective:
    - **File->Import...->General->Existing Projects into Workspace**
    - Press **Next**
    - Press **Browse...**
    - Choose **~/workspace-training/EclipseTraining/org.eclipse.tracecompass.training.example**
    - Make sure a single project is showing and is selected
    - Press **Finish**

- Right-click on project and choose **Team->Fetch from Upstream**

---
# How to reset to commits

- Open the History view (tab at the bottom of the screen)
- If nothing appears, click on the **org.eclipse.tracecompass.training.example**
- Right-click on the desired commit (**TRACECOMPASS2.1_START**), **Reset**, **Hard**

<center><img src="images/git_reset.png"/></center>

---
# How to test

- Start the "runtime" Eclipse

<center><image src="images/tracecompass_runtime-launch.png"/></center>

- Right-click on **training_ust_001**, Select Trace Type > Common Trace Format > LTtng UST Trace.
- Open the trace in Project Explorer (double-click)

<center><image style="width:250px; height:auto" src="images/tracecompass_trace_open.png"/></center>

---
# Exercise: Listen to a signal

- Reset to **TRACECOMPASS2.1_START** (should already have been done in previous steps)
- Create a class that will receive the signal, `EventReader`
- Instantiate the class. For now, we will do this in the `Activator` class.
- Register the new class with the `TmfTraceSignalManager`
- Create a `public` method that will receive the signal:
	- Annotate your method with `@TmfSignalHandler`
	- It needs a `TmfTraceOpenedSignal` parameter
	- Make it output something to the console (System.out.println)
- <b>Go!</b>

---
# Trace API

`ITmfTrace`

- One per trace, a central object in Trace Compass
- Knows about the key trace attributes: Number of events, file, trace type, etc.
- Allows you to seek at a location in a trace and get events one by one
- Does validation to determine whether or not a file is of this trace type (for
automatic detection, etc.)

<center><image src="images/tracecompass_tmftrace_class.png"/></center>

---
# Event Provider

- Event providers have the capability of handling event requests.
- Often implemented in pair with `ITmfTrace` in each trace type
- Of note, `getNext()` provides the next event in the trace

<center><image src="images/tracecompass_tmfeventprodider_class.png"/></center>

---
# Event Requests

`TmfEventRequest`

- Used to obtain series of events from an event provider (usually `ITmfTrace`)
- **Asynchronous**: Send the request and receive events one by one when they are done being parsed

~~~java
eventProvider.sendRequest(new TmfEventRequest(
        TmfEvent.class, 0, ITmfEventRequest.ALL_DATA,
        ITmfEventRequest.ExecutionType.BACKGROUND) {

    @Override
    public void handleData(ITmfEvent event) {
        super.handleData(event);
    }
});
~~~

---
# Events

`ITmfEvent`

- Represents a single event in the trace
- Contains a **name** (event type). Use `getName()` to retrieve it.
- Contains a **time stamp**. Use `getTimestamp()` to retrieve it.
- Contains **content** (fields). Use `getContent()` to retrieve it in the form of an `ITmfEventField`. Fields can then be retrieved with `getField("myfield")` for example. Fields can also have sub-fields

~~~java

ITmfEventField field = event.getContent().getField("myfield");
if (field != null) {
    System.out.println(field.getFormattedValue());
}
~~~

---
# Exercise: Read events from the trace

- Reset to **TRACECOMPASS2.2_START**
- In the signal handler, get the trace object from the signal parameter
- Send an event request to the trace:
	- Create an anonymous class of type `TmfEventRequest`
	- Override handleData and output the time stamp of each event to console
		- **Bonus**: Print a specific field of the event
	- When the request is **completed**, output something to the console (System.out.println)
- <b>Go!</b>

---
# Module 2 Review

- **Signals** can be used to react to different things.
- **ITmfTrace** is a central object that validates if files are of this trace type, provides trace attributes, seeks to locations and retrieves events.
- **Event Requests** are a mean to obtain a series of events asynchronously.
- **Events** have a name, a time stamp and content (fields). Fields can have sub-fields.
- In the exercises:
	- We have used **signals**
	- We have retrieved **events** using **event requests** sent to the **trace** (`ITmfTrace`)

---
# Module 3
## Analysis Framework

- Overview
- Analysis Module
- Plug-in extension point
- Analysis Requirements
- Analysis Parameter provider
- Dependent Analysis
- Analysis output

---
# Analysis Framework Overview
- API for integrating trace analyses
- Plug-in extension point
- Shows what can be done with trace content
- Provides hooks to add views
- Integrated with the Project Explorer
- Schedules analyses in Eclipse jobs (automatically or on demand)
- Manages dependencies between multiple analyses
- Manages requirements to execute analyses

---
# Analysis Module
- All analyses implement `ITmfAnalysisModule`
- Abstract implementation `TmfAbstractAnalysisModule`
- 0..N analyses per trace or experiment

~~~java
public class ProcessingTimeAnalysis extends TmfAbstractAnalysisModule {
	public ProcessingTimeAnalysis() {}
	@Override
	protected boolean executeAnalysis(IProgressMonitor monitor) 
		throws TmfAnalysisException {
		// TODO implement logic
		return true;
	}
	@Override
	protected void canceling() {
	}
}
~~~

---
# Analysis Module (2)
- Analysis is scheduled `IAnalysisModule#schedule()`
- `IAnalysisModule#waitForCompletion()` will block thread until completion
- Use Progress monitor in `executeAnalysis()` 
	- To monitor progress
	- Handle user cancellation (important!)
- An analysis can be cancelled using `IAnalysisModule#cancel()`
- Provide help for user using `IAnalysisModule#getHelpText()`
- `TmfAnalysisManager` keeps track on available analysis per trace type

---
# Plug-in Extension Point
## Analysis Module
- **Identifier**: org.eclipse.linuxtools.tmf.core.analysis
~~~dtd
		<!ELEMENT module (parameter , tracetype)*>
		<!ATTLIST module
		id                 CDATA #REQUIRED
		name               CDATA #REQUIRED
		analysis_module    CDATA #REQUIRED
		icon               CDATA #IMPLIED
		automatic          (true | false)
		applies_experiment (true | false) >
~~~
- **id**: The unique ID that identifies this analysis module.
- **name**: The trace analysis module's name as it is displayed to the end user
- **analysis_module**: Class that implements the `IAnalysisModule` interface.
- **icon**: The icon associated to this analysis module.
- **automatic**: Execute automatically when trace is open, or on request by users
- **applies_experiment**: If it applies to traces or experiments.

---
# Plug-in Manifest Editor
- Click on **Add...** Button
- Find org.eclipse.linuxtools.tmf.core.analysis
- Right click on org.eclipse.linuxtools.tmf.core.analysis -> New -> module
- Fill-in relevant data (id, name analysis_module)

<center><img src="images/ExtensionAnalysisModule.png" width="80%" height="80%"/></center>

---
# Project Explorer
<center><img src="images/ProjectExplorerWithAnalysis.png" width="40%" height="40%"/></center>
- Shows all available analyses under trace or experiment
- Note: Need to open trace to see available analyses

---
# Apply to Trace Type
- Define the trace type the analysis applies (or not)

~~~dtd
		<!ELEMENT tracetype EMPTY>
		<!ATTLIST tracetype
		class   CDATA #REQUIRED
		applies (true | false) >
~~~

- **class**: base trace class this analysis applies to or not 
	- Note - it also applies to traces extending this class
- **applies**: Does this tracetype element mean the class applies or not (default true)

---
# Apply to Trace Type (2)
- Right-click on analysis module -> New -> tracetype
- Fill-in the class

<center><img src="images/ExtensionAnalysisModule-TraceType.png" width="80%" height="80%"/></center>

---
# Exercise: Create an analysis module
- Reset to **TRACECOMPASS3.1_START**
- Add a new analysis module by adding an extension (plugin.xml)
	- extension point: `org.eclipse.linuxtools.tmf.core.analysis`
- New module (hint: right-mouse click on added extension)
	- _id_: `org.eclipse.tracecompass.training.example.processing.module`
	- _name_: `Processing Analysis`
	- click on hyperlink **analysis_module** 
		- Class name: `ProcessingTimeModule`
		- Select **Browse...** button and find superclass `TmfAbstractAnalysisModule`
		- Remove `IAnalysisModule` interface from Interfaces list
- Output something on console (in method `executeAnalysis()`)
- continue on next slide

---
# Exercise: Create an analysis module
- Right-click on Processing Analysis -> New -> tracetype
- Click on **Browse...** and find class `LttngUstTrace`
- Run Trace Compass and Open trace training_ust_001
- **Go!**	

---
# Exercise Review
## What we accomplished

- Defining an analysis extension
- Applying analysis to a trace type
- Implementing an analysis module class
- Running the analysis
- Exploring the integration in the Project Explorer

---
# Analysis Requirements
- Provide information to user if analysis can't run
- Requirements on event types or specific event field
- Implement interface `IAnalysisRequirementProvider`

~~~java
public interface IAnalysisRequirementProvider {
	Iterable&lt;TmfAbstractAnalysisRequirement> 
		getAnalysisRequirements();
}
~~~

---
# Analysis Requirements (2)
- Extend `TmfAbstractAnalysisRequirement` or
- Use existing classes 
	- `TmfAnalysisEventRequirement`: events by name
	- `TmfAnalysisEventFieldRequirement`: event fields for some or all events
	- `TmfCompositeAnalysisRequirement`: combine multiple ones
- Requirements have a priority 
	- e.g. `PriorityLevel#MANDATORY`, `PriorityLevel#OPTIONAL`

---
# Analysis Requirements Example
~~~java
@Override
public Iterable&lt;TmfAbstractAnalysisRequirement> getAnalysisRequirements() {
	Set&lt;TmfAbstractAnalysisRequirement> requirements = fAnalysisRequirements;
	if (requirements == null) {
	Set&lt;String> requiredEvents = ImmutableSet.of(
		"ust_master:CREATE",
		"ust_master:START",
	);
	// Initialize the requirements for the analysis: events
	TmfAbstractAnalysisRequirement eventsReq = 
		new TmfAnalysisEventRequirement(requiredEvents, 
										PriorityLevel.MANDATORY);
	requirements = ImmutableSet.of(eventsReq);
	fAnalysisRequirements = requirements;
	return requirements;
}
~~~

---
# Analysis Requirements
## Project Explorer

- If requirements are not fulfilled the analysis is strike-through
- Help text is available through context-sensitive menu

<center><img src="images/ProjectExplorer-no-requirements.png " width="75%" height="75%"/></center>

---
# Analysis Parameter Provider
- Analysis may have parameters
- Default values can be set as part of analysis extension
- Add parameter provider to analysis in plugin.xml file

~~~dtd
		<!ELEMENT parameterProvider (analysisId)>
		<!ATTLIST parameterProvider
		class CDATA #REQUIRED>
~~~

- **class**: The class that contains this analysis parameter provider.
	- Implement `IAnalysisParameterProvider`
	- Extend `TmfAbstractAnalysisParameterProvider`
- Use listener to register another view to be notified when selection changes

---
# Parameter Provider Example
~~~java
public class MyAnalysisParamProvider extends TmfAbstractAnalysisParamProvider {
	@Override
	public String getName() {
		return "My Analysis Provider";
	}
	@Override
	public Object getParameter(String name) {
		if (name.equals("ThreadId")) {
		return new Integer("1234");
		}
		return null;
	}
	@Override
	public boolean appliesToTrace(ITmfTrace trace) {
		return (trace instanceof LttngUstTrace);
	}
}
~~~

---
# Dependent Analyses
- An analysis can depend on other analyses
- Dependent analysis needs to execute beforehand
- Dependent analysis will be scheduled automatically
- Implement `TmfAbstractAnalysisModule#getDependentAnalyses()`

---
# Dependent Analyses (2)
~~~java
protected Iterable&lt;IAnalysisModule> getDependentAnalyses() {
	ITmfTrace trace = getTrace();
	if (trace == null) {
		return Collections.EMPTY_SET;
	}
	IAnalysisModule module = trace.getAnalysisModule(TidAnalysisModule.ID);
	if (module == null) {
	    return Collections.EMPTY_SET;
	}
	return ImmutableSet.of(module);
}
~~~

---
# Analysis Output
- Analysis can have one or more outputs
- Typically it's an Eclipse view
- All analysis outputs implement `ITmfAnalysisOutput`
- For Eclipse views, use class `TmfAnalysisViewOutput`
- Shown in Project Explorer under the traces

---
# Analysis Output (2)
- Associates an output with an analysis module or a class of analysis modules in plugin.xml

~~~dtd
	<!ELEMENT output (analysisId | analysisModuleClass)>
	<!ATTLIST output
		class CDATA #REQUIRED
		id    CDATA #IMPLIED>
~~~

- **class**: The class of this output.
- **id**: An ID for this output. For example, for a view, it would be the view ID.

---
# Plug-in Extension Point
## Plug-in Manifest Editor

- Right-click on `org.eclipse.linuxtools.tmf.core.analysis` -> New -> output
	- Fill in class of output and id of view
- Right-click on output -> New -> analysisModuleClass
	- Fill-in id of analysis

<center><img src="images/AnalysisOutputExtension.png" width="70%" height="70%"/></center>

---
# Project Explorer
- Shows all available view under the analyses
- Trace needs to open to see available analyses
- Analysis requirements need to be fulfilled 

<center><img src="images/ProjectExplorerWithOutput.png" width="30%" height="30%"/></center>

---
# Exercise: Create an output
- Reset to **TRACECOMPASS3.3_START**
- Create a Eclipse view (see Plug-in Development course)
	- _id_: `org.eclipse.tracecompass.training.example.processing.states`
	- _name_: Processing States
	- _class_: `ProcessingStatesView`
- Add an output
	- _class_: org.eclipse.tracecompass.tmf.ui.analysis.TmfAnalysisViewOutput
	- _id_: org.eclipse.tracecompass.training.example.processing.states
- Assign to analysis
	_class_: org.eclipse.tracecompass.training.example.ProcessingTimeAnalysis
- Run Trace Compass, open trace and view (from Project Explorer)
- **Go!**	

---
# Exercise Review
## What we accomplished

- Implementing analysis output
- Opening the output from the Project Explorer
- Exploring the analysis in Project Explorer

---
# Module 4
## Generic State System

- Generic State System Overview
- State System API
- State System Analysis Module

---
# Generic State System Overview
<center><img src="images/StateSystemOverview.png" width="50%" height="50%"/></center>

- Utility to track states over the duration of a traces
- State system abstracts events, analyzes traces and creates models to be displayed
- Persistent on disk, does not need to be rebuilt between runs
- Allows fast (O(log n)) queries of state attributes by time or type
- Support for several state systems in parallel

<center><img src="images/StateSystemKernelExample.png" width="70%" height="70%"/></center>

---
# State System Definitions
- **Attribute**
	- Smallest element of a state
	- Has a State Value
	- Changes over time
- **Attribute Tree:** 
	- Tree-like structure
	- Each attribute can have a value and sub-attributes
	- Access attributes using a path
- **Quark**:
	- Unique, constant identifier of an attribute (an integer)
	- Makes faster queries

---
# State System Definitions (2)
- **State Interval**
	- State intervals are returned when querying the state system

- **State History**
	- Storage container of all the state intervals
	- Backend determines how state history is stored
	- State history can be on disk or in memory 

- **Queries**
	- Queries return state intervals
	- Full query: Whole state of model for given timestamp
	- Single query: State of a particular attribute for the given timestamp

---
# Attribute Tree example
- Linux Kernel State System
- Example path: Threads/1001/PPID

~~~
	  |- CPUs
	  |  |- &lt;CPU-number> -> CPU Status
	  |     |- CURRENT_THREAD
	  |     |- SOFT_IRQS
	  |     |  |- &lt;Soft-IRQ-number> -> Soft IRQ Status
	  |     |- IRQS
	  |        |- &lt;IRQ-number> -> IRQ Status
	  |- Threads
	     |- &lt;Thread-number> -> Thread Status
	        |- PPID
	        |- EXEC_NAME
	        |- PRIO
	        |- SYSTEM_CALL
~~~

---
# State System API
- State value interface: `ITmfStateValue` (deprecated in favor of `Object` when possible)
- State interval interface: `ITmfStateInterval`
- Read and write interface to the state system: `ITmfStateSystemBuilder`
- Query interface: `ITmfStateSystem`
- Analysis using a state system: `TmfStateSystemAnalysisModule`
- All state provider implement `ITmfStateProvider`
	- Extend abstract class `AbstractTmfStateProvider`
- Create a state system and assign a backend: `StateSystemFactory`

---
# State Value Interface
- All state values implement interface `ITmfStateValue`
- Note - Use `Object` instead of `ITmfStateValue` where possible for better performance. Supported objects are: `Integer`, `Long`, `Double` or `String`.

~~~java
public enum Type {NULL, INTEGER, LONG, DOUBLE, STRING, CUSTOM;}
~~~

- Create a state value using state value factory `TmfStateValue`, for example:

~~~java
ITmfStateValue value = TmfStateValue.nullValue();
ITmfStateValue intValue = TmfStateValue.newValueInt();
ITmfStateValue longValue = TmfStateValue.newValueLong();
~~~

---
# State Value Interface
- Read the value, for example: `IntegerStateValue`

~~~java
ITmfStateValue value = getValue();
if (value.getType() == Type.Integer) {
	int retVal = value.unboxInt();
}

Object value2 = getValueObject();
if (value2 instanceof Integer) {
	int retVal = (Integer) value2;
}
~~~

---
# State Interval Interface
- All state intervals implement interface `ITmfStateInterval`
- Has a start and end time

~~~java
long getStartTime();
long getEndTime();
~~~

- Provides the quark and state value

~~~java
int getAttribute();
ITmfStateValue getStateValue();
Object getValue();
~~~

- Validates whether it intersects with a given timestamp

~~~
boolean intersects(long timestamp);
~~~

---
# Building a state system
## ITmfStateSystemBuilder

- Main interface used during state system building: `ITmfStateSystemBuilder`
- Getting/adding an attribute quark using an absolute path 
~~~java
int getQuarkAbsoluteAndAdd(String... path);
~~~
- Getting/adding an attribute quark using a relative path
~~~java
int getQuarkRelativeAndAdd(int startingNodeQuark, String... subPath);
~~~

---
# Building a state system (2)

## ITmfStateSystemBuilder

- Modifying a state value when state change occurs
  - Note - timestamp is a long value

~~~java
void modifyAttribute(long t, Object value, int attributeQuark)
	throws StateValueTypeException;
~~~

- Update an ongoing state value 
  - When getting value only at the end of the state
  - e.g. return value of a function call

~~~java
void updateOngoingState(Object newValue, int attributeQuark);
void updateOngoingState(ITmfStateValue newValue, int attributeQuark);
~~~

- Query an ongoing state value 
  - Get the current value of an attribute if needed while building the state system

~~~java
Object queryOngoing(int attributeQuark);
ITmfStateValue queryOngoingState(int attributeQuark);
~~~

---
# Building a state system (3)
## ITmfStateSystemBuilder

- Push and pop a state value on a stack

~~~java
void pushAttribute(long t, Object value, int attributeQuark)
	throws StateValueTypeException;
~~~

~~~java
Object popAttributeObject(long t, int attributeQuark)
	throws StateValueTypeException;
~~~

---
# State System Analysis Module
## TmfStateSystemAnalysisModule

- State system analysis modules typically extend `TmfStateSystemAnalysisModule`
- By default, full history on disk (default can be overwritten)
- Takes care of reading the trace using an event request
- Handles cancellation of analysis by user
- State systems are stored in hidden directory **.tracing** in workspace
	- `<workspace>/<project>/.tracing/<trace>/`
	- State system file is: `<analysis id>.ht`
	- Delete state systems: Right-click on trace -> **Delete Supplementary Files...**

---
# State System Analysis Module (2)
## TmfStateSystemAnalysisModule
- Access state system using static utility method

~~~java
ITmfStateSystem myStateSystem = TmfStateSystemAnalysisModule.getStateSystem(trace, "my.analysis.id");
~~~

- Create state provider class 

~~~java
@Override
protected ITmfStateProvider createStateProvider() {
	ITmfTrace trace = getTrace();
	if (trace == null) {
		throw new IllegalStateException();
	}
	return new ProcessingTimeStateProvider(trace);
}
~~~

---
# State provider
- All state provider implement interface `ITmfStateProvider`
- Typically, extend `AbstractTmfStateProvider`
	- Uses a buffering scheme to not block event request
- Implement `ITmfStateProvider#getVersion()`
	- To force recreation of state system change return value
- Implement `ITmfStateProvider#getNewInstance()`
- Implement `AbstractTmfStateProvider#eventHandle()`

---
# State provider (2)
~~~java
protected void eventHandle(ITmfEvent event) {
	final ITmfStateSystemBuilder stateSystem = getStateSystemBuilder();
	switch (event.getName()) {
	case IEventConstants.CREATE_EVENT:
		// get event field with name
		String requester = 
			event.getContent().getFieldValue(String.class, "requester");

		// get quark of attribute for path Requester/&lt;requester>
		int quark = stateSystem.getQuarkAbsoluteAndAdd("Requester", requester);

		// Create new state value
		Object stateValue = IEventConstants.ProcessingStates.INITIALIZING.ordinal();

		// get time of event
		long t = event.getTimestamp().getValue();

		// apply state change
		stateSystem.modifyAttribute(t, stateValue, quark);
		return;
	}
}
~~~

---
# Exercise: Implement a state provider
- Reset to **TRACECOMPASS4.1_START**
- Open `ProcessingTimeAnalysis`
	- Implement createStateProvider()
	- Return an instance of `ProcessingTimeStateProvider` (class already exists) 
- Implement `ProcessingTimeStateProvider`
	- See next slides for state machine and attribute tree
- Run Trace Compass and open trace
- Open State System Explorer (Window -> Show View -> Tracing -> State System Explorer)
	- Explore state system org.eclipse.tracecompass.training.example.ht 
	- Navigate events table and follow state changes
		- Hint: add search criteria (:CREATE|:START|:STOP|:END)
- **Go!**

---
# Exercise State Machine
<center><img src="images/ExerciseStateMachine.png" width="50%" height="50%"/></center>

- Note - When receiving **ust_master:end** set the state to the null state!

---
# Exercise Attribute Tree

~~~
	  |- Requester
	        |- &lt;requester> -> State Value
~~~

- Example path: Requester/&lt;requester&gt;
	- Where &lt;requester&gt; is taken from an event field of a CTF event.
- State values:
	- 0=INITIALIZING
	- 1=PROCESSING
	- 2=WAITING

---

# Bonus exercise: 2nd State Machine
- Reset to **TRACECOMPASS4.2_START**
- Update `ProcessingTimeStateProvider` (see state machine on next slide)
	- See next slides for state machine and attribute tree
- Run Trace Compass and open trace
- Open State System Explorer
	- Explore state system org.eclipse.tracecompass.training.example.ht 
	- Navigate events table and follow state changes
- **Go!**

---

# Example State Machine (2)
<center><img src="images/ExerciseProcessingStateMachine.png" width="45%" height="45%"/></center>

---
# Exercise Attribute Tree

~~~
|- Requester
      |- &lt;requester> -> State Value
            |-&lt;id>   -> State Value
~~~

- Example path: Requester/&lt;requester&gt;/&lt;id&gt;
	- Where &lt;requester&gt; is taken from event field of CTF event
- State values:
	- 0=INITIALIZING
	- 1=PROCESSING

---
# Exercise: Adding optional field

- Reset to **TRACECOMPASS4.3_START**
- Update `ProcessingTimeStateProvider`
  - Add field "value" from trace event to attribute "number"

```
|- Requester
      |- &lt;requester> -> State Value
            |- &lt;id>  -> State Value
                  |- number -> Number Value
```

---
# Exercise: Adding receiver

- Reset to **TRACECOMPASS4.4_START**
- Update `ProcessingTimeStateProvider`
  - Add attribute \<receiver> from `BALL_REQUEST_RECEIVE` field "receiver"
  - Set state value to the name of the requester from field "sender"
  - Set state value to `null` when `BALL_REPLY_SEND` is received

```
|- Receiver
      |- &lt;receiver> -> Requester Name
|- Requester
      |- &lt;requester> -> State Value
            |- &lt;id>  -> State Value
                  |- number -> Number Value
```

---

# Exercise Review

## What we accomplished

- Overview of Generic State System APIs (for building)
- Creating a state system analysis module
- Implementing a state system provider
- Exploring of a state system using the State System Explorer
- Deleting the supplementary files

---
# Query a state system
## ITmfStateSystem

- Main interface for accessing state system `ITmfStateSystem`
- Use after state system is built
- Throws an exception if attribute doesn't exist
- Getting a quark of an attribute from absolute path

~~~java
int getQuarkAbsolute(String... attribute)
	throws AttributeNotFoundException;
~~~

- Getting a quark from a relative path

~~~java
int getQuarkRelative(int startingNodeQuark, String... subPath)
	throws AttributeNotFoundException;
~~~

---
# Query a state system (2)
## ITmfStateSystem

- Getting a quark of an optional attribute from absolute path

~~~java
int optQuarkAbsolute(String... attribute);
~~~

- Getting a quark of an optional attribute from relative path

~~~java
int optQuarkRelative(int startingNodeQuark, String... subPath);
~~~
- Return `#INVALID_ATTRIBUTE` (-2) if it doesn't exist

---
# Query a state system (3)
## ITmfStateSystem

- Getting a list of quarks from a wildcarded path ("*" or "..")

~~~java
List&lt;Integer> getQuarks(String... pattern);
~~~

- Getting a list of quarks from a wildcarded path relatively ("*" or "..")

~~~java
List&lt;Integer> getQuarks(int startingNodeQuark, String... pattern);
~~~

- Waiting until a state system is built (with or without timeout)

~~~java
void waitUntilBuilt();
void waitUntilBuilt(long timeout);
~~~

---
# Query a state system (4)
## ITmfStateSystem

- Querying a single state at a given timestamp

~~~java
ITmfStateInterval querySingleState(long t, int attributeQuark)
	throws StateSystemDisposedException;
~~~


- Querying full state at a given timestamp

~~~java
List&lt;ITmfStateInterval> queryFullState(long t)
	throws StateSystemDisposedException;
~~~

---
# Query a state system (5)
## ITmfStateSystem

- Multiple attribute and multiple times iterable query
- Iteration of returned intervals is in undefined order and may contain duplicates

~~~java
Iterable&lt;ITmfStateInterval> query2D(@Collection&lt;Integer> quarks,
	Collection&lt;Long> times) 
		throws StateSystemDisposedException, IndexOutOfBoundsException,
		TimeRangeException;
~~~


- Multiple attribute and time range iterable query
- Iteration of returned intervals is in undefined order and may contain duplicates

~~~java
Iterable&lt;ITmfStateInterval> query2D(Collection&lt;Integer> quarks, 
	long start, long end)
		throws StateSystemDisposedException, IndexOutOfBoundsException,
		TimeRangeException;
~~~

---
# Query a state system (6)
## StateSystemUtils
- Utility class to query history range
- Getting all the states for a given quark between start and end time

~~~java
public static List&lt;ITmfStateInterval> queryHistoryRange(
	ITmfStateSystem ss, int attributeQuark, long t1, long t2)
		throws AttributeNotFoundException, StateSystemDisposedException
~~~

- Getting all the states for given a quark between start and end time with resolution

~~~java
public static List&lt;ITmfStateInterval> queryHistoryRange(
	ITmfStateSystem ss, int attributeQuark, long t1, long t2, long resolution, 
	IProgressMonitor monitor)
		throws AttributeNotFoundException, StateSystemDisposedException
~~~

---
# Exercise: Query a state system
- Reset to **TRACECOMPASS4.5_START**
- Open view class ProcessingStatesView
	- Implement TODOs to query sate system in method print states
	- Use `ITmfStateSystem` interface and utility `StateSystemUtils` 
	- Use method `outputText()` to print text on screen
- **Go!**	

---
# Exercise Review
## What we accomplished

- Understanding of state values and state intervals
- Getting attribute quarks
- Performing  single queries
- Performing full queries
- Querying a history range

---
# Module 5
## Time Graph Views

- Time Graph Viewer Overview
- Time Graph Viewer Model
- Time Graph Viewer API
- Time Graph View Overview
- Time Graph View API
- Searching and Filtering in Time Graph View

---
# Time Graph Viewer Overview
<center><img src="images/TimeGraphView-explained.png" width="100%" height="100%"/></center>

---
# Time Graph Viewer Overview
- Visualizes **states** over time
	- For example, processes, threads, cores, IRQs...
- Provides common features
	- Common **Time Axis** (`TimeGraphScale`)
	- **Marker Axis**	
	- **Navigation** with mouse, keyboard and toolbar buttons
	- **Zooming**
	- **Searching** (rows and states)
	- **Filtering** (rows and states)
	- **Highlighting** of regions of interest (makers or time selection)
- Supports drawing of **arrows** and **symbols**
- **Tree** structure that supports **columns**

---
# How to create a Time Graph viewer?
- Create a Time Graph viewer instance: `TimeGraphViewer`
- Define content provider to provide Time Graph Model root entries
- Define a presentation provider to define how to display states
- Define a filter content/label provider and columns for filter dialog
- Provide a Time Graph Model

~~~java
TimeGraphViewer viewer = new TimeGraphViewer();
viewer.setContentProvider(new MyTimeGraphContentProvider());
viewer.setPresentationProvider(new MyPresentationProvider);
viewer.setFilterLabelProvider(new MyFilterLabelProvider());
viewer.setFilterColumns(fFilterColumns);
viewer.setInput(getModel();
~~~

---
# Time Graph Model
<center><img src="images/TimeGraphView-Model.png" width="50%" height="50%"/></center>

---

# Time Graph Model (2)
## ITimeGraphEntry
<center><img src="images/TimeGraphEntries.png" width="70%" height="70%"/></center>

- All time graph models implement interface `ITimeGraphEntry`
- Typically models extend default implementation `TimeGraphEntry`
- It's a tree structure: ITimeGraphEntry has 0..\* `ITimeGraphEntry` children
- Using a content provider the root entries can be supplied for a model object

~~~java
ITimeGraphEntry getParent();
List&lt;ITimeGraphEntry> getChildren();
String getName();
boolean hasTimeEvents();
Iterator&lt;? extends ITimeEvent> getTimeEventsIterator();
~~~

---
# Time Graph Model (3)
## ITimeEvent

<center><img src="images/TimeEvents.png" width="50%" height="50%"/></center>

- Each `ITimeGraphEntry` has 0..\* more time events
- Time events define the state intervals to be displayed
- All time events implement interface `ITimeEvent`
- Typically time events extend default implementation `TimeEvent`

~~~java
ITimeGraphEntry getEntry();
long getTime();
long getDuration();
~~~

---
# Presentation Provider
- Provides the **colors** to be used for each time event
- Defines the **tooltip** to show when hovering over a time event
- Provides possibility to draw **overlays**
- Can customizes the entry **height**
- All presentation providers implement interface `ITimeGraphPresentationProvider`
- Typically presentation provider extends `TimeGraphPresentationProvider`

~~~java
StateItem[] getStateTable();
int getStateTableIndex(ITimeEvent event);
void postDrawEvent(ITimeEvent event, Rectangle bounds, GC gc);
Map<String, String> getEventHoverToolTipInfo(ITimeEvent event);
// ...
~~~

---
# Exercise: Create a Time Graph Viewer
- Reset to **TRACECOMPASS5.1_START**
- Open view class ProcessingStatesView
- In createPartControl()
	- create new instance of TimeGraphViewer
	- set presentation provider (`ProcessingStatesPresentationProvider`)
- Implement time graph model in method fillTimeGraph() (see TODOs)
- Run Trace Compass and explore the Time Graph Viewer features
- Question: What limitations do you foresee with this implementation?
- **Go!**

---
# Exercise Review
## What we accomplished

- Creating a Time Graph Viewer
- Setting a presentation provider
- Creating a time graph model from a state system
- Exploring of the Time Graph Viewer navigation

---
# Time Graph View Overview
- **Eclipse view** wrapping Time Graph Viewer 
- Common **abstract class** with reoccurring and reusable code
	- **Handles** and sends signals (e.g. trace opened, time range selected)
	- **Loads** the view content
	- **Provides** default set of buttons
	- Common synchronized **time axis** with other views
- Provides support for **lazy loading** of viewer
- Provides hooks to add overlay **markers**
- Support for link events, i.e. **arrows**
- Can interface with state systems

---
# Time Graph View Overview (2)
- Typically views extend abstract classes
	- `AbstractTimeGraphView`
	- `AbstractStateSystemTimeGraphView`
	- `BaseDataProviderTimeGraphView` (**preferred**)
- Use `AbstractTimeGraphView` to populate each row at a time
	- Loads only visible rows
	- Works with or without state systems
- Use `AbstractStateSystemTimeGraphView` to populate all entries by time
	- High number of rows
	- Uses full state system queries (more efficient query than single query)
	- Works only with state systems
- Use `BaseDataProviderTimeGraphView` to populate using Data Provider API
	- Decoupled UI and core code
	- Possible to serialize data structure from data provider

---
# Class Hierarchy
<center><img src="images/AbstractTimeGraphView.png" width="80%" height="80%"/></center>

---
# Time Graph View API
## AbstractTimeGraphView (build thread)

- **Build thread**:
	- Build list of **time graph entries** (rows)
	- Build list of **time events** for each time graph entry for **whole trace** range
		- To display coarse events before **lazy loading** of zoom window is complete
	- Called from base class for each trace in **experiment** (parentTrace)
	- Adding root entries to view using `AbstractTimeGraphView#addToEntryList()`

~~~java
protected abstract void buildEntryList(ITmfTrace trace, 
	ITmfTrace parentTrace, IProgressMonitor monitor);
~~~

---
## AbstractTimeGraphView (zoom thread)

- **Zoom thread**:
	- Build **time event** list for each time graph entry
	- Per **zoom** level and display **resolution**

~~~java
protected abstract List&lt;ITimeEvent> getEventList(
	TimeGraphEntry entry, long startTime, long endTime, 
	long resolution, IProgressMonitor monitor);
~~~

---
## AbstractTimeGraphView (arrows)

- Creates **arrows** between time graph entries for a given start and end time
- Computed for the current zoom window
- Providing a list of linked events implementing interface `ILinkEvent`

~~~java
protected List&lt;ILinkEvent> getLinkList(long startTime, 
	long endTime, long resolution, 
	IProgressMonitor monitor);
~~~

- List will be propagated to TimeGraphViewer object

---
## AbstractTimeGraphView (markers)

- Markers are **overlays** in the viewer
- Shown also on **marker axis**
- Separated by marker category
- Bookmarks set by user in Time Graph View or externally
- Common **trace markers** can be defined per trace type
- View specific markers can be defined directly in the view class:
~~~java
protected List&lt;String> getViewMarkerCategories();
protected List&lt;IMarkerEvent> getViewMarkerList(long startTime, 
	long endTime, long resolution, 
	IProgressMonitor monitor);
~~~

---
## AbstractTimeGraphView (filtering and searching)

- Possibility to provide **filter** for **time graph entries** (rows)
- Filter dialog uses a regular tree viewer 
  - Providing columns names and Label provider (extends TreeLabelProvider)
  - Optional, providing a content provider if needed 

~~~java
String[] filterColumns = { "Entry" };
setFilterColumns(filterColumns);
setFilterLabelProvider(new FilterLabelProvider());
~~~

- **Searching** for **time graph entries** is built-in:
  - Use key shortcut **CTRL+F**

---
## BaseDataProviderTimeGraphView API

- All APIs of `AbstractTimeGraphView` above are handled and delegated to a data provider
  - Except filter columns and filter label provider, that still need to be set
  - The data provider must implement `ITimeGraphDataProvider`
  - Optional interfaces `IOutputAnnotationProvider`, `IOutputStyleProvider`
- Only need to provide the data provider ID in constructor
- Use `BaseDataProviderTimeGraphPresentationProvider`
  - It queries the `IOutputStyleProvider` to fetch the style map for the legend

```java
public class MyTimeGraphView extends BaseDataProviderTimeGraphView {
	public MyTimeGraphView() {
		super("my.view.id", new BaseDataProviderTimeGraphPresentationProvider(), MyDataProvider.ID);
		setFilterColumns(new String[] {...});
		setFilterLabelProvider(new TreeLabelProvider() {...});
	}
}
```

---
# AbstractTimeGraphDataProvider API
- Create the data provider factory and declare it in plugin.xml `org.eclipse.tracecompass.tmf.core.dataprovider` extension to associate it to the data provider ID
- Create the data provider implementation
  - Implement getTree() to return the tree of rows
  - Implement getRowModel() to return the time graph states of selected rows for a given window range
  - Implement fetchArrows() to return the arrows for a given window range
  - Implement fetchTooltip() to return custom tool tip information for the given state, arrow or annotation
  - Implement fetchAnnotationCategories() to return the list of supported categories and fetchAnnotations() to return the symbols on selected rows and/or time markers for a given window range
  - Implement fetchStyle() to return the list of base styles that can be referred to in the output model elements and configured in the legend

---
## AbstractTimeGraphDataProvider (tree)

```java
protected abstract TmfTreeModel&lt;M> getTree(ITmfStateSystem ss, Map&lt;String, Object> fetchParameters,
			@Nullable IProgressMonitor monitor) throws StateSystemDisposedException;
```

- Input `fetchParameters`
  - none
- Output `TmfTreeModel`
  - `List<String> headers`: Column header names of tree on left of time graph
  - `List<ITmfTreeDataModel> entries`: Each element represents a row in the time graph. Order of entries is significant.
    - `long id`: unique number (in scope) of this row
    - `long parentId`: parent id or `-1` for a root
    - `String name` and/or `List<String> labels`: Entry name / column values of tree on left of time graph
    - `OutputElementStyle style`: Optional, currently used for default style of time event gaps
    - `boolean hasRowModel`: Indicates if the row has time events
  - `String scope`: Optional, if used, indicates a common scope for shared row id between multiple data providers
- This can be called multiple times if the state system is being built while the view is shown.
- The data provider should keep track of its allocated entry ids. They should be reused for the same entries on subsequent calls, and can be requested to identify specific entries in the other API methods.
- The typical implementation is to query a state system's attribute tree and to create entries associated with their corresponding state system attribute.

---
## AbstractTimeGraphDataProvider (row model)

```java
protected abstract @Nullable TimeGraphModel getRowModel(ITmfStateSystem ss,
			Map&lt;String, Object> parameters, @Nullable IProgressMonitor monitor)
			throws StateSystemDisposedException;
```

- Input `parameters`
  - `REQUESTED_TIME_KEY : List<Long>`: list of timestamps to be sampled, short states that do not intersect these can be omitted in the response
  - `REQUESTED_ITEMS_KEY : List<Long>`: list of requested entry ids
  - `REGEX_MAP_FILTERS_KEY : Map<Integer, Collection<String>>`: Optional, map of filter expressions, key is a `IFilterProperty`
  - `FULL_SEARCH_KEY : Boolean`: Optional, if true then the full time range between the first and last requested timestamp should be searched for filter matches, but only one matching state per gap in requested timestamps need be returned in the response

----
## AbstractTimeGraphDataProvider (row model cont.)

- Output `TimeGraphModel`
  - `List<ITimeGraphRowModel> rows`: Each element contains the states of a specific row in the time graph. The order of rows is insignificant.
    - `long entryId`: unique number (in scope) of this row
    - `List<ITimeGraphState>`: list of time graph states in chronological order
      - `long startTime`: start time of the state
      - `long duration`: duration of the state
      - `String label`: Optional, state label
      - `int value`: Optional, can be used to indicate state style if a specific time graph presentation provider is used. Set to `Integer.MIN_VALUE` when not used.
      - `OutputElementStyle style`: Optional, indicates the state style if the default data provider time graph presentation provider is used.
      - `Multimap<String, Object> metadata`: Contains unspecified information that can matched against filter expressions, and can also be used by specific implementations.
      - `int activeProperties`: Bitmap of active `IFilterProperty` that can be used to describe the state
- This is called every time a new window range is set, when scrolling makes new entries visible, when filter is changed, etc.
- Should be as efficient as possible, usually by using a 2D query on the state system after computing the state system attributes that relate to the requested items.

---
## ITimeGraphDataProvider (arrows)

```java
TmfModelResponse&lt;List&lt;ITimeGraphArrow>> fetchArrows(Map&lt;String, Object> fetchParameters,
			@Nullable IProgressMonitor monitor);
```

- Input `fetchParameters`
  - `REQUESTED_TIME_KEY : List<Long>`: list of timestamps to be sampled, arrows that do not intersect these can be omitted in the response
- Output `List<ITimeGraphArrow>`
  - `List<ITimeGraphArrow>`: Each element represents an arrow in the time graph. The order is insignificant.
    - `long sourceId`: unique id of the entry at the start of the arrow
    - `long destinationId`: unique id of the entry at the end of the arrow
    - `long startTime`: time at the start of the arrow
    - `long duration`: time difference between end and start of the arrow
    - `int value`: Optional, can be used to indicate arrow style if a specific time graph presentation provider is used. Set to `Integer.MIN_VALUE` when not used.
    - `OutputElementStyle style`: Optional, indicates the arrow style if the default data provider time graph presentation provider is used.
    - `Multimap<String, Object> metadata`: Contains unspecified information that can matched against filter expressions, and can also be used by specific implementations.
    - `int activeProperties`: Bitmap of active `IFilterProperty` that can be used to describe the arrow

----
## ITimeGraphDataProvider (arrows cont.)
- This is called every time a new window range is set, etc.
- There is no requested items list because arrows may cross the visible window going between entries that are not visible.

---
## ITimeGraphDataProvider (tooltip)

```java
TmfModelResponse&lt;Map&lt;String, String>> fetchTooltip(Map&lt;String, Object> fetchParameters,
			@Nullable IProgressMonitor monitor);
```

- Input `fetchParameters`
  - `REQUESTED_TIME_KEY : List<Long>`: singleton list of the hover time (state) or timestamp of the tooltip element (arrow or annotation)
  - `REQUESTED_ITEMS_KEY : List<Long>`: singleton list of the entry id of the tooltip element (source id for arrow)
  - `REQUESTED_ELEMENT_KEY : IOutputElement`: model of the element whose tooltip is fetched (state, arrow or annotation)
- Output `<Map<String, String>`
  - `<Map<String, String>`: Map of <name, value> of tooltip information. The order is significant.
- This is called when the user hovers over an element in the timegraph.
- It can also be called when the user invokes the context menu over an element in the timegraph.

---
## IOutputAnnotationProvider (annotations)

```java
TmfModelResponse&lt;AnnotationCategoriesModel> fetchAnnotationCategories(Map&lt;String, Object> fetchParameters,
			@Nullable IProgressMonitor monitor);
```

- Input `fetchParameters`
  - none
- Output `AnnotationCategoriesModel`
  - `List<String> annotationCategories`: Name of supported annotation categories
- This is called every time a new window range is set, when scrolling makes new entries visible, when filter is changed, etc.
- The annotation categories are used to build the view's **Show Markers** menu

```java
TmfModelResponse&lt;AnnotationModel> fetchAnnotations(Map&lt;String, Object> fetchParameters,
			@Nullable IProgressMonitor monitor);
```

- Input `fetchParameters`
  - `REQUESTED_TIME_KEY : List<Long>`: list of timestamps to be sampled, annotations that do not intersect these can be omitted in the response
  - `REQUESTED_ITEMS_KEY : List<Long>`: list of requested entry ids for annotations attached to specific entries

----
## IOutputAnnotationProvider (annotations cont.)
- Output `AnnotationModel`
  - `Map<String, Collection<Annotation>> annotations`: Map of annotations per category
    - `AnnotationType type`: `CHART` or `TREE` to indicate on which part of the time graph the annotation should be visible
    - `long startTime`: start time of the annotation (for `CHART` annotations)
    - `long duration`: duration of the annotation (for `CHART` annotations)
    - `long entryId`: unique id of the entry to which the annotation is attached, or `-1` if it is not attached.
    - `String label`: the label of the annotation
    - `OutputElementStyle style`: Optional, indicates the annotation style if the default data provider time graph presentation provider is used.
    - `Multimap<String, Object> metadata`: Contains unspecified information that can matched against filter expressions, and can also be used by specific implementations.
    - `int activeProperties`: Bitmap of active `IFilterProperty` that can be used to describe the annotation
- This is called every time a new window range is set, etc.

---
# Style API

## IOutputStyleProvider (styles)

```java
TmfModelResponse&lt;OutputStyleModel> fetchStyle(Map&lt;String, Object> fetchParameters, @Nullable IProgressMonitor monitor);
```

- Input `fetchParameters`
  - none
- Output `OutputStyleModel`
  - `Map<String, OutputElementStyle>`: Map of style per style key
    - `String parentStyleKey`: Optional, usually `null`
    - `Map<String, Object> styleValues`: Map of style properties, keys and values are defined in `StyleProperties`
- This is called at view creation and when the presentation provider is refreshed (legend is modified by user)
- The style keys returned here are meant to be referenced by the `OutputElementStyle` of output elements (states, arrows, annotations and series) in the other APIs.
- The style key is unique to this data provider.
- The `parentStyleKey` is usually not set in the styles defined here, unless a style extends another style defined in the same map.
- The styles returned here are also used to build the legend of the view that uses this data provider, and these styles are configurable by the user.
- A data provider is free to use its own styles for its output elements without first defining them here. In that case they do not appear in the legend and are not configurable.

---
## Output Element Style

- The style of an output element (state, arrow, annotation or series) is indicated by including an `OutputElementStyle` in its model
  - `OutputElementStyle(styleKey)`: The style for this style key is defined in the map returned by the `IOutputStyleProvider`
  - `OutputElementStyle(null, styleMap)`: The style properties are explicitly defined in the included style map
  - `OutputElementStyle(parentStyleKey, styleMap)`: The style extends a defined style, the style map properties extend or override the parent style's properties
- The styles can be hierarchical, i.e. the parent style can extend another parent style.
- The style key supports multiple inheritance with comma-separated style keys, e.g. `"styleKey1,styleKey2"`if these are defined style keys. The last style properties extend or override the previous.
- Some style properties have defaults, they do not need to be included in the map if the default behaviour is expected

---
## Style Properties

The supported style properties (with their default value) for each output element type are:

- State
  - `BACKGROUND_COLOR` (#000000)
  - `OPACITY` (1.0f)
  - `HEIGHT` (1.0f)
  - `BORDER_STYLE` (`NONE`)
  - `BORDER_COLOR` (#000000) (if there is a border style)
  - `BORDER_WIDTH` (1) (if there is a border style)
- Line (time graph line entry)
  - `COLOR` (#000000)
  - `OPACITY` (1.0f)
- Arrow
  - `COLOR` (#000000)
  - `OPACITY` (1.0f)
  - `WIDTH` (1)
- Annotation
  - `SYMBOL_TYPE` (`NONE`) (for symbol annotation)
  - `HEIGHT` (1.0f) (for symbol annotation)
  - `COLOR` (#000000)
  - `OPACITY` (1.0f)

----
## Style Properties (cont.)

- Series
  - `SERIES_TYPE` (`LINE`)
  - `SERIES_STYLE` (`SOLID`) (for line type)
  - `WIDTH` (1) (for line type)
  - `SYMBOL_TYPE` (`NONE`) (for scatter type)
  - `COLOR` (#000000)
  - `OPACITY` (1.0f)

The common style properties for each style is:

- `STYLE_NAME`: Name of style in the legend, if not set then the style key is used as name
- `STYLE_GROUP`: Group name that style belongs to in the legend

---
## Style Modifiers

Color style properties (`COLOR`, `BACKGROUND_COLOR` and `BORDER_COLOR`) can be modified by adding a `BLEND` modifier before the property in the style hierarchy. The modifier should be appended to the base property to create the property key. The blend modifier is an RGBA value in hex format.

```Java
styleMap.put("state", new OutputElementStyle(null,
	ImmutableMap.of(StyleProperties.BACKGROUND_COLOR, "#00FF00")));
OutputElementStyle darkerState = new OutputElementStyle("state",
	ImmutableMap.of(StyleProperties.BACKGROUND_COLOR + StyleProperties.BLEND, "#0000003F")); // blend 25% black over parent color
```

Number style properties (`HEIGHT`, `WIDTH`) can be modified by adding a `FACTOR` modifier before the property in the style hierarchy. The modifier should be appended to the base property to create the property key. The factor modifier is a Float value.

```java
styleMap.put("arrow", new OutputElementStyle(null,
	ImmutableMap.of(StyleProperties.WIDTH, 1)));
OutputElementStyle thickerArrow = new OutputElementStyle("arrow",
	ImmutableMap.of(StyleProperties.WIDTH + StyleProperties.FACTOR, 2.0f)); // double the width of parent
```

The purpose of implementing this using modifiers instead of just setting the modified style's property to the target computed value directly is that the modified style will adapt to customisation of the base style by the user in the legend.

---
# Create a Data Provider Time Graph View

<center><img src="images/ProcessingStatesView.png" width="100%" height="100%"/></center>

- Root is trace name
- Master is from Receiver/\<receiver> attribute
- Challenger is from Requester/\<requester> attribute
  - States are INITIALIZING/PROCESSING/WAITING from this attribute's value
- 0/1/2/3 are from Requester/\<requester>/\<id> attributes
  - States are INITIALIZING/PROCESSING from this attribute's value
  - PROCESSING state on \<id> has a Value tooltip based on Requester/\<requester>/\<id>/number attribute
- Blue circle (ball) annotation on start and end of \<receiver> interval
- Green arrow (ball request) from start of \<receiver> interval to start of \<requester> interval
- Red arrow (ball reply) from end of \<requester> interval to end of \<receiver> interval
- Legend has 3 styles groups: States/Arrows/Markers

---
## Data Provider Time Graph View skeleton
~~~java
public class ProcessingStatesView extends BaseDataProviderTimeGraphView {
	public ProcessingStatesView() {
		// TODO: Call super with view ID, presentation provider and data provider ID
		// TODO: Enable entry filtering by setting filter columns and label provider
	}
}
~~~

```java
public class ProcessingStatesDataProviderFactory implements IDataProviderFactory {
	@Override
	public @Nullable ITmfTreeDataProvider&lt;? extends ITmfTreeDataModel> createProvider(@NonNull ITmfTrace trace) {
		// TODO: Get the analysis module for the trace
		// TODO: Schedule the analysis
		// TODO: Create and return a data provider instance
	}
}
```

```xml
<extension
        point="org.eclipse.tracecompass.tmf.core.dataprovider">
    <dataProviderFactory
        class=""
        id="">
        <!--TODO: add data provider factory class-->
        <!--TODO: add data provider ID-->
    </dataProviderFactory>
</extension>
```

----
## Data Provider Time Graph View skeleton (cont.)
```java
public class ProcessingStatesDataProvider extends AbstractTimeGraphDataProvider&lt;@NonNull ProcessingStatesAnalysisModule,
	@NonNull TimeGraphEntryModel> implements IOutputAnnotationProvider, IOutputStyleProvider {

	// Constructor
	public ProcessingStatesDataProvider(@NonNull ITmfTrace trace, @NonNull ProcessingStatesAnalysisModule module) {
		super(trace, module);
	}

	@Override
	protected TmfTreeModel&lt;@NonNull TimeGraphEntryModel> getTree(@NonNull ITmfStateSystem ss,
			Map&lt;String, Object> parameters, @Nullable IProgressMonitor monitor) throws StateSystemDisposedException {
        // TODO: Create a root entry for the trace
        // TODO: Get all the &lt;receiver> attributes from the state system
        // TODO: Get an id and create an entry for each &lt;receiver>
        // TODO: Get all the &lt;requester> attributes from the state system
        // TODO: Get an id and create an entry for each &lt;requester>
        // TODO: Get all the &lt;id> child attributes of each &lt;requester>
        // TODO: Get an id and create an entry for each &lt;id>
        // TODO: Return the list of all created entries
	}
```

----
## Data Provider Time Graph View skeleton (cont.)
```java
	@Override
	protected @Nullable TimeGraphModel getRowModel(@NonNull ITmfStateSystem ss,
			@NonNull Map&lt;@NonNull String, @NonNull Object> parameters, @Nullable IProgressMonitor monitor)
			throws StateSystemDisposedException {
		// TODO: Extract the requested timestamps and ids from the parameters
        // TODO: Get the ids to quarks map for the requested ids
        // TODO: Compile the list of quarks needed to get state data from state system
        // TODO: Do a 2D query of the state system and collect the returned intervals in a tree multimap per quark
        // TODO: For each id/quark pair, get the list of intervals
        // TODO: For each interval, create a time graph state model and add to a list
        // TODO: Create a time graph row model with this list
        // TODO: Return the list of all created time graph row models
	}

	@Override
	public @NonNull TmfModelResponse&lt;@NonNull List&lt;@NonNull ITimeGraphArrow>> fetchArrows(
			@NonNull Map&lt;@NonNull String, @NonNull Object> fetchParameters, @Nullable IProgressMonitor monitor) {
		// TODO: Extract the requested timestamps from the parameters
        // TODO: Compile the list of quarks needed to get arrow data from state system
        // TODO: Do a 2D query of the state system and collect the returned intervals in a tree multimap per quark
        // TODO: For each quark, get the list of intervals
        // TODO: For each interval, get source id, destination id, start and duration
        // TODO: Create a time graph arrow model and add to a list
        // TODO: Return the list of all created time graph arrow models
	}
```

----
## Data Provider Time Graph View skeleton (cont.)
```java
	@Override
	public @NonNull TmfModelResponse&lt;@NonNull Map&lt;@NonNull String, @NonNull String>> fetchTooltip(
			@NonNull Map&lt;@NonNull String, @NonNull Object> fetchParameters, @Nullable IProgressMonitor monitor) {
		// TODO: Extract the requested timestamp, id and element from the parameters
        // TODO: Get the quark associated to that id
        // TODO: Query the state system for the interval value
        // TODO: Add a name/value pair to a tooltip map
        // TODO: Return the map of tooltips
	}
```

----
## Data Provider Time Graph View skeleton (cont.)
```java
	@Override
	public @NonNull TmfModelResponse&lt;@NonNull AnnotationCategoriesModel> fetchAnnotationCategories(
			@NonNull Map&lt;@NonNull String, @NonNull Object> fetchParameters, @Nullable IProgressMonitor monitor) {
		// TODO: Return the list of annotation categories
	}

	@Override
	public TmfModelResponse&lt;AnnotationModel> fetchAnnotations(
			@NonNull Map&lt;@NonNull String, @NonNull Object> fetchParameters, @Nullable IProgressMonitor monitor) {
		// TODO: Extract the requested timestamps and ids from the parameters
        // TODO: Get the ids to quarks map for the requested ids
        // TODO: Compile the list of quarks needed to get annotation data from state system
        // TODO: Do a 2D query of the state system and collect the returned intervals in a tree multimap per quark
        // TODO: For each id/quark pair, get the list of intervals
        // TODO: For each interval, create an annotation model and add to a map of annotations per category
        // TODO: Return the map of annotations
	}
```

----
## Data Provider Time Graph View skeleton (cont.)
```java
	@Override
	public @NonNull TmfModelResponse&lt;@NonNull OutputStyleModel> fetchStyle(
			@NonNull Map&lt;@NonNull String, @NonNull Object> fetchParameters, @Nullable IProgressMonitor monitor) {
        // TODO: Assign a unique style key to each style and add to a map of style per key
		// TODO: Create and add state styles, set BACKGROUND_COLOR, STYLE_NAME, STYLE_GROUP
        // TODO: Create and add arrow styles, set COLOR, WIDTH, STYLE_NAME, STYLE_GROUP
        // TODO: Create and add annotations styles, set SYMBOL_TYPE, COLOR, HEIGHT, STYLE_NAME, STYLE_GROUP
        // TODO: Make sure the state, arrow and annotation model elements use one of these style keys for their style
        // TODO: Return the map of styles
	}
}
```

---
# Exercise: Create a Time Graph View
- Reset to **TRACECOMPASS5.2_START**
- Open view class ProcessingStatesView
  - Make view extend BaseDataProviderAbstractTimeGraphView
  - Implement constructor
    - Call super constructor with view ID, presentation provider and data provider ID
    - Set filter column names and label provider
- Open data provider factory class ProcessingStatesDataProviderFactory
  - Get the analysis module and create a data provider instance
- Open plugin.xml
  - Add an extension for "org.eclipse.tracecompass.tmf.core.dataprovider" to define the factory
- Open data provider class ProcessingStatesDataProvider
  - Implement getTree()
  - Implement getRowModel()
  - Implement fetchArrows()
  - Implement fetchTooltip()
  - Implement fetchAnnotationCategories() and fetchAnnotations()
  - Implement fetchStyle()
- Run Trace Compass and explore the Time Graph View features
- Question: What are the differences to the previous exercise?
- **Go!**

---
# Exercise Review
## What we accomplished 

- Extending the BaseDataProviderAbstractTimeGraphView and AbstractTimeGraphDataProvider
- Understanding concept of UI, build and zoom thread
	- Implementing fetchStyle(), fetchAnnotationCategories() -&gt; used in UI thread (init)
	- Implementing getTree() -&gt; used in build thread
	- Implementing getRowModel(), fetchArrows(), fetchAnnotations() -&gt; used in zoom thread
	- Implementing fetchTooltip() -&gt; used in UI thread (when user hovers an element)
- Exploring of the Time Graph View

---
# Module 6
## Timing Analysis

---
# Timing Analysis
## Concept

- We have two metrics to analyse, what is the data and **when** did it come.
- Every event has **time** info (time stamp), let's take advantage of that.
- Measure time between a **start** and **end** state
	- Simple: Start and end **event**
	- Often: State Machine to determine start and end
	- **Start** and **End** time : <span style="border-style:solid; border-width: 5px;">**Segment**</span>


- Represent Execution times, latencies, latency chains etc.

---
# Timing Analysis
## Why?

- Locate timing problems
- Analyse timing problems
- Find root cause and solution
- Potential delays (find problem before it occurs)
- Difficult to debug if it happens sporadically

---
# Timing Analysis
## Example

- System Call Latency, e.g. futex

<center><img src="images/timing_systemcalls.png"/></center>

---
# Timing Analysis
## Example

- IRQ Latency

<center><img src="images/timing_irq.png"/></center>

---
# Timing Analysis
## Example

- High Resolution Timer – cyclictest application of rt-tests
- Latency between timer expiry until task starts

<center><img src="images/timing_latencychain.png"/></center>

- Latency = Δ1+ Δ2 + Δ3
	- Event 1: Timer expires
	- Event 2: Interrupt handler marks the task to react
	- Event 3: Linux scheduler switches to the task
	- Event 4: Application task begins executing

---
# Timing Analysis
## Generalization

<center>
<div style="display:table-cell; width:50%;"><img style="width:400px; height:auto" src="images/timing_states.png"/></div>
<div style="display:table-cell; width:50%; text-align: left; vertical-align: middle">
<ul>
<li>Time between start and end</li>
<li>Time for each transition</li>
<li>Percentage sub-duration vs total</li>
</ul>
</div>
<br/>
</center>
---
# Timing Analysis
## Using states

- State machine for timing analysis
	- Implementation in Java as Trace Compass extension

- Data-driven pattern matching (in XML)
	- Defining timing analyses on-the-fly

---
# Timing Analysis API
## ISegment

- A segment provides a **start time** and an **end time**
- What that duration means is specific to the analysis that will generate it
- This interface is the basis of the "Timing" family of analysis and views

<center><img src="images/timing_segment.png"/></center>

---
# Timing Analysis API
## ISegmentStore

- Stores segments (`ISegment`)
- Extends "well known" Java `Collection` interface
	- `iterator()`, `add()`, `isEmpty()`, etc.
- Adds the notion of intersection: `getIntersectingElements`(start, end)

<center><img src="images/timing_segmentstore.png"/></center>

---
# Timing Analysis API
## Limitations

- Segment store can be in memory or on disk
- If your trace will generate many segments, use `SegmentStoreType.OnDisk`
- **Warning**: The segment store table becomes slow when having huge amount of segments. This is due
to the SWT implementation of the virtual table in Linux

- This will likely be fixed in the near future versions of Trace Compass.

---
# Timing Analysis API
## AbstractSegmentStoreAnalysisModule

- Similar to normal analysis but with the notion of segments
- **Builds** segments and **stores** them: `buildAnalysisSegments()`
- Provides the segment store (for views, etc.): `getSegmentStore()`

<center><img src="images/timing_segmentanalysis.png"/></center>

---
# Exercise: Create segment store module

- Reset to **TRACECOMPASS6.1_START**
- `ProcessingLatencyModule` is created for you with some **TODOS**. plugin.xml is already filled.
	- Populate the list of aspects (columns) to be used by some views. **Hint**: Reused some of the existing classes in the package.
	- Create the analysis request. This class has the responsibility of filling the segment store.
	- Complete the **processEvent** helper method. This should handle events one by one and adds segments to the segment store. Note that we need two event (start and end) so we need to keep track of ongoing ones. 
	- Return all the list of aspects to be used: `Name` and `Content`

---
# Module 6 Review

- **Segments** (`ISegment`) are defined by a start and an end time. They can represent things like execution times, latencies, etc.
- Multiple segments can represent **latency chain**. For example multiple sub-steps of a process.
- State systems can help generalize the creation of segments
- **ISegmentStore** stores segments like a Java Collection but with intersection methods
- **AbstractSegmentStoreAnalysisModule** helps building segments, storing them and providing them to clients (views).
- In the exercise: We have created a **segment-based analysis module** that creates segments and stores them in a segment store which will be available for our views.

---
# Module 7
## Timing Analysis Views

---
# Timing Analysis Views
## Overview

- Several types of views can be added that make use of segment store analysis
	- Latencies (table)
	- Statistics
	- Scatter chart
	- Density

- Most of those views follow a similar pattern:
	- An abstract viewer to help create the widgets specific to the type of view
	- An abstract view class to help create the container this type of view and that will create the chosen viewer
	- This is similar to Time Graph views VS Time Graph Viewer

---
# Timing Analysis Views
## Overview

- Viewers query analysis based on segment stores (see previous module!)

<center><img src="images/timingviews_getstore.png"/></center>

---
# Latency Table view

- The Latency view displays segments in a simple table format.

<center><img src="images/timingviews_latencies.png"/></center>
	(Example based on System Calls analysis)

---
# Latency Table view
## API

- Use declarative-only API in plug-in.xml for default view 
- Or implement `AbstractSegmentStoreTableView` and `AbstractSegmentStoreTableViewer` and hook it up in plug-in.xml

---
# Latency Table view (2)
## API

- Use declarative-only API in plug-in.xml for default view 
	- class: `SegmentStoreTableView`
	- For view ID and analysis output ID:`<viewID>:<analysisModuleId>`
	where
		- viewID: `org.eclipse.tracecompass.analysis.timing.ui.segstore.table`
		- analysisModuleId: Segment store analysis module ID

---
# Latency Table view (3)
## API

- `AbstractSegmentStoreTableView`
	- An abstract class that helps create a table view.
	- `createSegmentStoreViewer`: has to return an AbstractSegmentStoreTableViewer
- `AbstractSegmentStoreTableViewer`
	- An abstract class that helps create a table viewer.
	- `createProviderColumns`: can be overridden to have greater influence on columns (order, etc.).
	- `getSegmentStoreProvider`: returns which analysis module will provide the segment store

~~~java
@Override
protected ISegmentStoreProvider getSegmentStoreProvider(ITmfTrace trace) {
    return TmfTraceUtils.getAnalysisModuleOfClass(trace, SystemCallLatencyAnalysis.class, SystemCallLatencyAnalysis.ID);
}
~~~

---
# Exercise: Create a Latency Table

- Reset to **TRACECOMPASS7.1_START**
- Create class `ProcessingLatencyTableViewer`, select super class (extends)  `AbstractSegmentStoreTableViewer`
	- Implement method `getSegmentStoreProvider`
- In plugin.xml, create the missing class `ProcessingLatencyTableView` (tip: click the hyperlink to bring up the New Class wizard). Select super class (extends)  `AbstractSegmentStoreTableView`.
	- Implement `createSegmentStoreViewer` in `ProcessingLatencyTableView`
- <b>Go!</b>

---
# Statistics view

- The Statistics view displays statistics for each segment type
	- You can also navigate to the minimum and maximum of each segment type from this view (eg. longest `futex` system call)

<center><img src="images/timingviews_statistics.png"/></center>

---
# Statistics view (2)
## API

- Use declarative-only API in plug-in.xml for default view
	- Only total statistic and statistic per name (see `INamedSegment` interface')
- Or implement API classes and hook it up in the plug-in.xml
	- `AbstractSegmentStatisticsAnalysis`
	- `AbstractSegmentStoreStatisticsView`
	- `AbstractSegmentStoreStatisticsViewer` 

---
# Statistics view (3)
## API

- Use declarative-only API in plug-in.xml for default view
	- class: `SegmentStoreStatisticsView`
	- For view ID and analysis output ID:`<viewID>:<analysisModuleId>`
	where
		- viewID: `org.eclipse.tracecompass.analysis.timing.ui.segstore.statistics`
		- analysisModuleId: Segment store analysis module ID

---
# Statistics view (4) - TODO update
## API 

- Non-declarative way
- `AbstractSegmentStatisticsAnalysis`
	- An abstract class that helps create a statistics module reusing an existing segment store provider (i.e. another module).
	- `getSegmentType`: returns the segment type to compute the statistics for.
	- `getSegmentProviderAnalysis`: returns an existing segmentstore provider.

~~~java
@Override
protected ISegmentStoreProvider getSegmentProviderAnalysis(ITmfTrace trace) {
    return TmfTraceUtils.getAnalysisModuleOfClass(trace, SystemCallLatencyAnalysis.class, SystemCallLatencyAnalysis.ID);
}
~~~

---
# Statistics view (5) - TODO update
## API

- `AbstractSegmentsStatisticsView`
	- An abstract class that helps create a statistics view.
	- `createSegmentStoreStatisticsViewer`: has to return an `AbstractSegmentsStatisticsViewer`

- `AbstractSegmentsStatisticsViewer`
	- An abstract class that helps create a statistics viewer.
	- `createStatisticsAnalysiModule`: returns which analysis module will provide the statistics.
	- `updateElements`: creates and updates items in the statistics tree. This is where a hierarchy can be created.

---
# Exercise: Create a Statistics View

- Reset to **TRACECOMPASS7.2_START**
- In plugin.xml, create view extension and use class `SegmentStoreStatisticsView`.
	- Fill the correct view ID
- In plugin.xml create analysis output extension for latency analysis module
- <b>Go!</b>

---
# Scatter chart - TODO update picture

- The Scatter view displays the segment durations over time in a 2D plot chart
	- Each dot represents the time it ended on the X-axis and its duration on the Y-axis
	- Makes it possible to spot **outliers**

<center><img src="images/timingviews_scatter.png"/></center>

---
# Scatter chart
## API

- `TmfChartView`
	- An abstract class that helps create a view based on a chart (SWTChart)
	- `createChartViewer`: has to return an `TmfXYChartViewer` (which `AbstractSegmentStoreScatterChartViewer` extends).
	- `createLeftChildViewer`: return an instance of `AbstractSegmentStoreScatterChartTreeViewer`
- `AbstractSegmentStoreScatterChartViewer`
	- An abstract class that helps create a scatter viewer.
	- Implement constructor to pass correct latency analysis module ID

---
# Exercise: Create a Scatter View

- Reset to **TRACECOMPASS7.3_START**
- Create class `ProcessingLatencyScatterGraphViewer`, select super class (extends)  `AbstractSegmentStoreScatterChartViewer`
	- Implement method constructor and pass `ProcessingLatancyAnalysis.ID`
- In plugin.xml, create the missing class `ProcessingLatencyScatterView` (tip: click the hyperlink to bring up the New Class wizard). Select super class (extends)  `TmfChartView`.
	- Implement `createChartViewer` 
	- Implement `createLeftChildViewer`
- <b>Go!</b>

---
# Density view

- The Density view displays the segment durations on the frequency domain.
	- Each bar represents the **duration** of the segment on the **X-axis** and the **count** of segments on the **Y-axis**
<center><img src="images/timingviews_density.png" style="width:600px"/></center>
<br/>
- In other words, fast system calls are on the left and slow system calls are on the right

---
# Density view
## API

- `AbstractSegmentStoreDensityView`
	- An abstract class that helps create a density view
	- `createSegmentStoreTableViewer`: has to return an `AbstractSegmentStoreTableViewer` (can reuse the one from Latency table view!). Left part of the view.
	- `createSegmentStoreDensityViewer`: has to return an `AbstractSegmentStoreDensityViewer`. Right part of the view.

- `AbstractSegmentStoreDensityViewer`
	- An abstract class that helps create a density viewer.
	- `getSegmentStoreProvider`: returns which analysis module will provide the segment

---
# Exercise: Create a Density View

- Reset to **TRACECOMPASS7.4_START**
- Create class `ProcessingLatencyDensityViewer`, select super class (extends)  `AbstractSegmentStoreDensityViewer`
	- Implement method `getSegmentStoreProvider`
- In plugin.xml, create the missing class `ProcessingLatencyDensityView` (tip: click the hyperlink to bring up the New Class wizard). Select super class (extends)  `AbstractSegmentStoreDensityView`.
	- Implement `createSegmentStoreTableViewer` : Reuse the one from Latency Table view!
	- Implement `createSegmentStoreDensityViewer`
- <b>Go!</b>

---
# Module 7 Review

- Many views can be built by reading a **segment store**.
- In the exercises, we have created:
	- **Latency Table** view
	- **Statistics** view
	- **Scatter chart** view
	- **Density** view
- Those views can be generated by follow a similar patter of creating a **viewer inside a view**, and specifying the segment store to use.

---
# Module 8
## Custom Analysis

---
# XML analysis basic
- The joys of XML analysis
	- Customize Trace Compass without recompiling
	- Add custom analysis
	- Add custom and reusable views
	- Share analysis and views
	- Find an execution flow within the trace

---
# XML analysis basic
## Advanced pattern matching
- Find **stateful** sequence within the trace


<center><img src="images/xml/general_pattern.png" width="60%" height="60%"/></center>

---
# XML analysis basic
## Advanced pattern matching
- XML description

~~~xml
<fsm id="process:processing">
	<precondition event="ust_master:PROCESS_START"/>
	<precondition event="ust_master:PROCESS_END"/>
	<initialState>
		<transition event="ust_master:PROCESS_INIT" 
			target="INITIALIZING" action="process_init:save_id"/>
	</initialState>
	<state id="INITIALIZING">
		<transition event="ust_master:PROCESS_START" cond="test_id" 
			target="PROCESSING" action="process_start"/>
	</state>
	<state id="PROCESSING">
		<transition event="ust_master:PROCESS_END" cond="test_id" 
			target="END" action="process_end"/>
	</state>
	<final id="END"/>
</fsm>
~~~

---
# XML analysis basic
## Generic views

<center>
<div style="display:table-cell; width:50%;"><img style="width:300px; height:auto" src="images/xml/tracecompass_xml_timegraph.png"/></div>
<div style="display:table-cell; width:50%; text-align: center; vertical-align: middle"><span>XML **Time graph view**</span></div>
<br/>
<div style="display:table-cell; width:50%;"><img style="width:300px; height:auto" src="images/xml/tracecompass_xml_xy_chart.png"/></div>
<div style="display:table-cell; width:50%; text-align: center; vertical-align: middle"><span>XML **XY View**</span></div>
<br/>
<div style="display:table-cell; width:50%;"><img style="width:300px; height:auto" src="images/xml/tracecompass_timing_analysis.png"/></div>
<div style="display:table-cell; width:50%; text-align: center; vertical-align: middle"><span>**Timing analysis** on-the-fly</span></div>
</center>
---
# How do we use an XML analysis?
<center>
<div style="display:table-cell; width:50%; text-align: center; vertical-align: middle"><span>**Define** the analysis</span></div>
<div style="display:table-cell; width:50%;"><img style="width:300px; height:auto" src="images/xml/design_xml.png"/></div>
<br/>
<div style="display:table-cell; width:50%; text-align: center; vertical-align: middle"><span>**Import** an XML analysis</span></div>
<div style="display:table-cell; width:50%;"><img style="width:300px; height:auto" src="images/xml/manage_xml_analysis.png"/></div>
<br/>
<div style="display:table-cell; width:50%; text-align: center; vertical-align: middle"><span>**Analyze** result in Trace Compass</span></div>
<br/>
</center>
---
# Exercise: Importing the XML analysis
- Reset to commit **TRACECOMPASS8.1_START**
- In the project explorer,
    - Expand the tracing project
    - Right-click the Traces folder
    - Select `Manage XML analyses...`
    - In the opened dialog import the `training-example-full-states.xml` file and close the dialog.
- The analysis with the name `Processing Analysis XML` is now installed
- The analysis is present under the trace _training_ust_001_

---
# Exercise: Importing the XML analysis
<center><img src="images/xml/ManageXMLAnalysis.png" width="70%" height="70%"/></center>

---
# Exercise: Execute the XML analysis
- Open the trace.
	- The `Processing Analysis XML` analysis should be now expandable.

<center><img src="images/xml/analysis_and_view_under_trace.png" width="30%" height="30%"/></center>

- Expand the analysis. Several views are present under it.
- Open the view named `Processing States XML`

---
# Exercise: Observe the analysis 
- A timegraph view opens and is `populated`

<center><img src="images/xml/empty_entry.png" width="30%" height="30%"/></center>

- Some **entries (rows) are empty**. Why?
- **The state system is probably not well designed**
- Let's take a look at the XML file

---
# Exercise: Edit the analysis
- Reopen the **Manage XML analyses...** dialog (seen previously)
- Select **training-example-full-states** and click on the 'Edit' button

<center><img src="images/xml/edit_button.png" width="30%" height="30%"/></center>

- The XML file opens in an editor

---
# Exercise: Edit the analysis
- We can see that there are some `stateChange` where the  `stateValue` is set to **null**.
~~~xml
<stateValue type="null" />
~~~

- We probably don't want to set the value to **null** for the `ust_master:PROCESS_INIT` and `ust_master:PROCESS_START` events.
<br><br>
- Let's edit the XML file

---
# Exercise: Edit the analysis
- Change the **stateValue** for event `ust_master:PROCESS_INIT` to `$INITIALIZING`
~~~xml
    <stateValue type="int" value="$INITIALIZING"/>
~~~

- Change the **stateValue** for event `ust_master:PROCESS_START` to `$PROCESSING`
~~~xml
    <stateValue type="int" value="$PROCESSING"/>
~~~

- Save the file. The open trace should close. 
- Reopen the trace and the view

---
# Exercise: Edit the analysis
<center><img src="images/xml/filled_entries.png" width="40%" height="40%"/></center>

- The view is populated. There is **no empty entry**.

---
# Exercise review
- In the exercise, we have
	- Imported an XML analysis
	- Edited the XML analysis
	- Executed the XML analysis and analyzed the data

---
# Exercise: Timing Analysis
- Reset to commit **TRACECOMPASS8.2_START**
- Import the **training-example-processing-timing.xml** file

<center><img src="images/xml/manager_timing_analysis.png" width="40%" height="40%"/></center>

- The analysis with the name `Processing Latency XML` is now installed
- The analysis is present under the trace _training_ust_001_

---
# Timing Analysis
- Reopen The trace
    - Several views are now present under the analysis.
    
<left><img src="images/xml/latency_view_label.png" width="30%" height="30%"/></left>

- Open all 4 latency views

---
# Exercise: Observe the analysis
- All the views are empty
	- **The analysis probably does not create any latency data**
<br><br>
- Let's take a look at the XML file

---
# Exercise: Edit the analysis
- Open the XML file (using Edit)
- The file contains an action that creates latency data (segments) in the file
~~~xml
    <action id="processing_endeded">
        <segment>
          <segType segName="PROCESSING" />
          <segContent>
            <segField name="requester" type="string">
              <stateValue type="eventField" value="requester" />
            </segField>
            <segField name="id" type="string">
              <stateValue type="eventField" value="id" />
            </segField>
          </segContent>
        </segment>
      </action>
~~~
- **This action is never used**
<br>
- Let's edit the XML file

---
# Exercise: Edit the analysis
- We need to call the action when the processing ended (when we receive the `ust_master:PROCESS_END` event).
- Let's add an action to the `ust_master:PROCESS_END` event transition.

~~~xml
<transition event="ust_master:PROCESS_END" target="end" cond="cond_same_data" action="processing_endeded" />
~~~

- Save the file. The open trace should close. 
- Reopen the trace and the latency views
- The views are now **populated**.
- The latency views and content are the same as the Java analysis

---
# Exercise: Analysis result
<center><img src="images/xml/latency_views_populated.png" width="70%" height="70%"/></center>

---
# Exercise Review
- In the exercise, we have:
	- Generated latency data using XML analysis
	- Analyzed latencies based on XML analysis
	- Visualized latency data in various latency views 

---
# Module 8 review
## What we have seen

- In this module, we have:
	- Learned how to `import an XML analysis`
	- Learned how to `execute the analysis` and analyze the data
	- Learned how to `edit the analysis`
	- Learned how to `generate latencies data` from the XML analysis
	- Learned how to `analyze latencies` based on XML analysis
