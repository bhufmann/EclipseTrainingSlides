% title: Eclipse Trace Compass
% title_class:                      #empty, largeblend[123] or fullblend
% subtitle: Extending Trace Compass for trace analysis
% subtitle_class:
% title_slide_class:
% title_slide_image:
% author: Bernd Hufmann
% author: Marc-André Laperle
% thankyou_blend: largeblend3       #largeblend[123] or fullblend
% thankyou_details:
% mail: Bernd.Hufmann@ericsson.com
% phone: +46 10 xx xx xxx
% sms: +46 72 xx xx xxx
% lync: Bernd.Hufmann@ericsson.com
% footer: (C) Ericsson AB
% footer: 2016-10-10
% logoslide: false
% useBuilds: true
% animate: false         #animate logoslide (chrome only)
% aspect_ratio: 16:9     #16:9, 16:10 or 4:3

---
title: Module 3
subtitle: Analysis Framework

- Overview
- Analysis Module
- Plug-in extension point
- Analysis Requirements
- Analysis Parameter provider
- Dependent Analysis
- Analysis output

---

title: Analysis Framework Overview
subtitle: 

- API for integrating trace analyses
- Plug-in extension point
- Shows what can be done with trace content
- Provides hooks to add views
- Integrated with the in Project Explorer
- Schedules analyses in Eclipse jobs (automatically or on demand)
- Manages dependencies between multiple analyses
- Manages requirements to execute analyses

TODO: picture

---
title: Analysis Module
subtitle:

- API for data collection
- Can have dependent analysis and requirements on trace content
- All analyses implement <code>ITmfAnalysisModule</code>
- Abstract implementation <code>TmfAbstractAnalysisModule</code>
- 0..N analyses per trace or experiment

	<pre class="prettyprint" data-lang="java">
	public class ProcessingTimeAnalysis extends TmfAbstractAnalysisModule {
		public ProcessingTimeAnalysis() {}
		@Override
		protected boolean executeAnalysis(IProgressMonitor monitor) throws TmfAnalysisException {
			return true;
		}
		@Override
		protected void canceling() {
		}
	}
    </pre>

---
title: Analysis Module (2)
subtitle: 

- Analysis is scheduled <code>IAnalysisModule#schedule()</code>
- <code>IAnalysisModule#waitForCompletion()</code> will block thread until completion
- Use Progress monitor in <code>executeAnalysis()</code> 
	- to monitor progress
	- handle user cancellation (important!)
- An analysis can be cancelled using <code>IAnalysisModule#cancel()</code>
- Provide help for user using <code>IAnalysisModule#getHelpText()</code>
- <code>TmfAnalysisManager</code> keeps track on available analysis per trace type
	- Can be queried to find specific analyses
	- Uses <code>IAnalysisModuleHelper</code> which is created per analysis module

---
title: Plug-in Extension Point
subtitle: Analysis Module
content_class: smaller

- Identifier: org.eclipse.linuxtools.tmf.core.analysis

	<pre class="prettyprint" data-lang="DTD">
		<!ELEMENT module (parameter , tracetype)*>
		<!ATTLIST module
		id                 CDATA #REQUIRED
		name               CDATA #REQUIRED
		analysis_module    CDATA #REQUIRED
		icon               CDATA #IMPLIED
		automatic          (true | false)
		applies_experiment (true | false) >
		</pre>

- _id_: The unique ID that identifies this analysis module.
- _name_: The trace analysis module's name as it is displayed to the end user
- _analysis_module_: The fully qualified name of a class that implements the IAnalysisModule interface.
- _icon_: The icon associated to this analysis module.
- _automatic_: Whether to execute this analysis automatically when trace is opened, or wait for the user to ask for it
- _applies_experiment_: If it applies to traces or experiments.

---
title: Plug-in Manifest Editor
subtitle: 

- Click on **Add...** Button
- Find org.eclipse.linuxtools.tmf.core.analysis
- Right click on org.eclipse.linuxtools.tmf.core.analysis -> New -> module
- Fill-in relevant data (id, name analysis_module)

<center><img src="images/ExtensionAnalysisModule.png" width="80%" height="80%"/></center>

---
title: Project Explorer
subtitle: 

- Shows all available analyses under trace or experiment
- Note: Need to open trace to see available analyses

<center><img src="images/ProjectExplorerWithAnalysis.png" width="40%" height="40%"/></center>

---
title: Exercise: Create an analysis
subtitle: 

- Reset to **TRACE_COMPASS_???**
- Add a new analysis module by adding an extension (plugin.xml)
	- extension point: <code>org.eclipse.linuxtools.tmf.core.analysis</code>
- New module (hint: right-mouse click on added extension)
	- _id_: <code>org.eclipse.tracecompass.training.example.processing.module</code>
	- _name_: <code>Processing Analysis</code>
	- click on hyperlink **analysis_module** 
		- Class name: <code>ProcessingTimeModule</code>
		- Select **Browse...** button and find superclass <code>TmfAbstractAnalysisModule</code>
		- Remove <code>IAnalysisModule</code> interface from Interfaces list
- Output something on console (in <code>executeAnalysis()</code>)
- **Go!**	

---
title: Exercise: Review
subtitle: 

- Defining an analysis extension
- Implementing an analysis module class
- Running the analysis
- Exploring the integration in the Project Explorer

---

title: Apply to Trace Type
subtitle: 

- Define the trace type the analysis applies (or not)

	<pre class="prettyprint" data-lang="DTD">
		<!ELEMENT tracetype EMPTY&>
		<!ATTLIST tracetype
		class   CDATA #REQUIRED
		applies (true | false) >
	</pre>

- 
- _class_: base trace class this analysis applies to or not 
	- Note: it also applies to traces extending this class
- _applies_: Does this tracetype element mean the class applies or not (default true)

---

title: Apply to Trace Type (2) 
subtitle: 

- Right-click on analysis module -> New -> tracetype
- Fill-in the class

<center><img src="images/ExtensionAnalysisModule-TraceType.png" width="80%" height="80%"/></center>

---

title: Exercise: Apply to trace type
subtitle: 

- Reset to **TRACE_COMPASS_???**
- Right-click on Processing Analysis -> New -> tracetype
- Click on **Browse...** and find class <code>LttngUstTrace</code>
- Run Trace Compass and
	- Open trace training_ust_001
	- Open trace TODO
- Compare the list of analyses of both traces
- **Go!**	

---
title: Exercise: Review
subtitle: 

- Applying analysis to a trace type
- Exploring the analysis in Project Explorer
	- Analysis shown for corresponding trace type
	- Analysis not shown for other trace type 
- Exploring the integration in the Project Explorer

---
title: Analysis Requirements
subtitle: 

- 
- Provide information to user if analysis can't run
- Requirements on event types or specific event field
- Implement interface <code>IAnalysisRequirementProvider</code>

	<pre class="prettyprint" data-lang="java">
	public interface IAnalysisRequirementProvider {
		Iterable&lt;TmfAbstractAnalysisRequirement&gt; getAnalysisRequirements();
	</pre>

- Extend <code>TmfAbstractAnalysisRequirement</code> or
- Use existing classes 
	- <code>TmfAnalysisEventRequirement</code>: events by name
	- <code>TmfAnalysisEventFieldRequirement</code>: event fields for some or all events
	- <code>TmfCompositeAnalysisRequirement</code>: combine multiple ones
- Have a priority e.g. <code>PriorityLevel#MANDATORY</code>, <code>PriorityLevel#OPTIONAL</code>

---
title: Analysis Requirements Example
subtitle: 

- 

	<pre class="prettyprint" data-lang="java">
	@Override
	public Iterable&lt;TmfAbstractAnalysisRequirement&gt; getAnalysisRequirements() {
		Set<TmfAbstractAnalysisRequirement> requirements = fAnalysisRequirements;
		if (requirements == null) {
		Set<String> requiredEvents = ImmutableSet.of(
			"ust_master:CREATE",
			"ust_master:START",
		);
		// Initialize the requirements for the analysis: events
		TmfAbstractAnalysisRequirement eventsReq = new TmfAnalysisEventRequirement(requiredEvents, PriorityLevel.MANDATORY);
			requirements = ImmutableSet.of(eventsReq);
			fAnalysisRequirements = requirements;
		}
		return requirements;
	}
	</pre>

---
title: Exercise: Add Analysis Requirements
subtitle: 

- Reset to **TRACE_COMPASS_???**
- Open class <code>ProcessingTimeAnalysis</code> 
- Override method <code>getAnalysisRequirements()</code>
- Create an <code>TmfAnalysisEventRequirement</code> for event names
	- Mandatory event names: ust_master:CREATE, ust_master:START, ust_master:STOP, ust_master:END, ust_master:PROCESS_INIT, ust_master:PROCESS_START, ust_master:PROCESS_END
- Return Iterable over the analysis requirements
- Run Trace Compass and explore the Project Explorer
	- What happens if one event name is missing?
	- Explore the help (context-sensitive menu) in that case 
- **Go!**

---
title: Exercise: Review
subtitle: 

- Implementing analysis requirement for event names
- Providing the analysis requirement from the analysis 
- Exploring the analysis in Project Explorer
	- Analysis shown if all requirements are fulfilled
	- Otherwise analysis is striked-through
- Showing the analysis help  

---

title: Analysis Parameter Provider
subtitle:

- Analysis may have parameters
- Default values can be set as part of analysis extension
- Add parameter provider to analysis in plugin.xml file

	<pre class="prettyprint" data-lang="DTD">
		<!ELEMENT parameterProvider (analysisId)>
		<!ATTLIST parameterProvider
		class CDATA #REQUIRED>
	</pre>

- _class_: The class that contains this analysis parameter provider.
	- Implement <code>IAnalysisParameterProvider</code>
	- Extend <code>TmfAbstractAnalysisParameterProvider</code>
- Use listener to register another view to be notified when selection changes

---
title: Parameter Provider Example
subtitle:

- 
	<pre class="prettyprint" data-lang="java">
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
	</pre>

---

title: Dependent Analyses 
subtitle:

- An analysis can depend on other analyses
- Dependent analysis need to execute beforehand
- Dependent analysis will be scheduled automatically
- Implement TmfAbstractAnalysisModule#getDependentAnalyses()

	<pre class="prettyprint" data-lang="java">
	protected Iterable<IAnalysisModule> getDependentAnalyses() {
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
	</pre>

---

title: Analysis Output
subtitle: 

- Analysis can have one or more outputs
- Typically it's an Eclipse view
- All analysis outputs implement <code>ITmfAnalysisOutput</code>
- For Eclipse views, use class <code>TmfAnalysisViewOutput</code>
- Shown in Project Explorer under the traces
- Associates an output with an analysis module or a class of analysis modules in plugin.xml

	<pre class="prettyprint" data-lang="DTD">
	<!ELEMENT output (analysisId | analysisModuleClass)>
	<!ATTLIST output
	class CDATA #REQUIRED
	id    CDATA #IMPLIED>
	</pre>

- 
- _class_: The class of this output.
- _id_: An ID for this output. For example, for a view, it would be the view ID.

---
title: Plug-in Extension Point
subtitle: Plug-in Manifest Editor

- Right-click on <code>org.eclipse.linuxtools.tmf.core.analysis</code> -> New -> output
	- Fill in class of output and id of view
- Right-click on output -> New -> analysisModuleClass
	- Fill-in id of analysis

<center><img src="images/AnalysisOutputExtension.png" width="70%" height="70%"/></center>

---
title: Exercise: Create an output
subtitle: 

- Reset to **TRACE_COMPASS_???**
- Create a Eclipse view (see Plug-in Development course)
	- _id_: <code>org.eclipse.tracecompass.training.example.processing.states</code>
	- _name_: Processing States
	- _class_: <code>ProcessingStatesView</code>
- Add an output
	- _class_: org.eclipse.tracecompass.tmf.ui.analysis.TmfAnalysisViewOutput
	- _id_: <code>org.eclipse.tracecompass.training.example.processing.states</code>
- Assign to analysis
	_class_: <code>org.eclipse.tracecompass.training.example.ProcessingTimeAnalysis</code>
- Run Trace Compass, open trace and view (from Project Explorer)
- **Go!**	

---
title: Exercise: Review
subtitle: 

- Implementing analysis output
- Opening the output from the Project Explorer
- Exploring the analysis in Project Explorer

---

title: Module 4
subtitle: Generic State System

- Generic State System Overview
- State System API
- State System Analysis Module

---

title: Generic State System Overview
subtitle: 

<center><img src="images/StateSystemOverview.png" width="50%" height="50%"/></center>

- Utility to track states over the duration of a traces
- State system abstracts events, analyzes traces and creates models to be displayed
- Persistent on disk, does not need to be rebuilt between runs
- Allows fast (O(log n)) queries of state attributes by time or type
- Support for several state systems in parallel

<center><img src="images/StateSystemKernelExample.png" width="70%" height="70%"/></center>

---

title: State System Definitions
subtitle:

- Attribute
	- Smallest element of a state
- Attribute Tree: 
	- Tree-like structure
	- Each attribute can have a value and sub-attributes
	- Access attributes using a path
- Quark:
	- Unique, constant identifier of an attribute
	- Makes faster queries
- State Value:
	- Value of the attribute
	- Changes over the time of the trace

---
title: State System Definitions (2)
subtitle:

- State Interval
	- State intervals are returned when querying the state system

- State History
	- Storage container of all the state intervals
	- State system backend determines how state history is stored
	- State history can be on disk or in memory 

- Queries
	- Queries return state intervals
	- Full query: give the whole state of model for given timestamp
	- Single querY: Returns the state of a particular attribute for the given timestamp

---

title: Attribute Tree example
subtitle:

- Linux Kernel State System
- Example path: Processes/1001/PPID

	<pre>
	  |- CPUs
	  |  |- &lt;CPU number&gt; -&gt; CPU Status
	  |  |  |- CURRENT_THREAD
	  |  |  |- SOFT_IRQS
	  |  |  |  |- &lt;Soft IRQ number&gt; -&gt; Soft IRQ Status
	  |  |  |- IRQS
	  |  |  |  |- &lt;IRQ number&gt; -&gt; IRQ Status
	  |- THREADS
	  |  |- &lt;Thread number&gt; -&gt; Thread Status
	  |  |  |- PPID
	  |  |  |- EXEC_NAME
	  |  |  |- PRIO
	  |  |  |- SYSTEM_CALL
	  </pre>


---

title: State System API
subtitle: 

- All state provider implement <code>ITmfStateProvider</code>
	- Extend abstract class <code>AbstractTmfStateProvider</code>
- Create a state system and assign a backend: <code>StateSystemFactory</code>
- Read and write interface to the state system: <code>ITmfStateSystemBuilder</code>
- Query interface: <code>ITmfStateSystem</code>
- State value interface: <code>ITmfStateValue</code>
- State interval interface: <code>ITmfStateInterval</code>

---

title: Building a state system
subtitle: ITmfStateSystemBuilder

- Main interface used during state system building: <code>ITmfStateSystemBuilder</code>
- Getting/adding an attribute quark using an absolute path 

	<pre class="prettyprint" data-lang="java">
	int getQuarkAbsoluteAndAdd(String... attribute);
    </pre>

- Getting/adding an attribute quark using a relative path

	<pre class="prettyprint" data-lang="java">
	int getQuarkRelativeAndAdd(int startingNodeQuark, String... subPath);
    </pre>

---

title: Building a state system (2)
subtitle: ITmfStateSystemBuilder

- Modifying a state value when state change occurs
- Note: timestamp is a long value

	<pre class="prettyprint" data-lang="java">
	void modifyAttribute(long t, @NonNull ITmfStateValue value, int attributeQuark)
		throws StateValueTypeException;
	</pre>

- Update an ongoing state value 
	- When getting value only at the end of the state
	- e.g. return value of a function call

	<pre class="prettyprint" data-lang="java">
	void updateOngoingState(@NonNull ITmfStateValue newValue, int attributeQuark);
	</pre>

---

title: Building a state system (3)
subtitle: ITmfStateSystemBuilder

- Push and pop a state value on a stack

	<pre class="prettyprint" data-lang="java">
	void pushAttribute(long t, @NonNull ITmfStateValue value, int attributeQuark)
		throws StateValueTypeException;
	</pre>

	<pre class="prettyprint" data-lang="java">
	ITmfStateValue popAttribute(long t, int attributeQuark)
		throws StateValueTypeException;
	</pre>

---
title: Query a state system
subtitle: ITmfStateSystem

- Main interface for accessing state system <code>ITmfStateSystem</code>
- Use after state system is build
- Throws an exception if attribute doesn't exist
- Getting a quark of an attribute from absolute path

	<pre class="prettyprint" data-lang="java">
    int getQuarkAbsolute(String... attribute)
            throws AttributeNotFoundException;
	</pre>

- Getting a quark from a relative path

	<pre class="prettyprint" data-lang="java">
	int getQuarkRelative(int startingNodeQuark, String... subPath)
		throws AttributeNotFoundException;
	</pre>

---
title: Query a state system (2)
subtitle: ITmfStateSystem

- Getting a quark of an optional attribute from absolute path
- returns <code>#INVALID_ATTRIBUTE</code> (-2) if it doesn't exist

	<pre class="prettyprint" data-lang="java">
    int optQuarkAbsolute(String... attribute)
            throws AttributeNotFoundException;
	</pre>

- Getting a quark of an optional attribute from relative path
- returns <code>#INVALID_ATTRIBUTE</code> (-2) if it doesn't exist

	<pre class="prettyprint" data-lang="java">
	int optQuarkRelative(int startingNodeQuark, String... subPath)
		throws AttributeNotFoundException;
	</pre>

---
title: Query a state system (3)
subtitle: ITmfStateSystem

- Getting a list of quarks from a wildcarded path ("*" or "..")

	<pre class="prettyprint" data-lang="java">
    List<Integer> getQuarks(String... pattern);
	</pre>

- Getting a list of quarks from a wildcarded path relatively ("*" or "..")

	<pre class="prettyprint" data-lang="java">
    List<Integer> getQuarks(int startingNodeQuark, String... pattern);
	</pre>

- Wait until a state system is build (with or without timeout)

	<pre class="prettyprint" data-lang="java">
	void waitUntilBuilt();
	void waitUntilBuilt(long timeout);
	</pre>

---
title: Query a state system (4)
subtitle: ITmfStateSystem

- Query a single state at a given timestamp

	<pre class="prettyprint" data-lang="java">
	ITmfStateInterval querySingleState(long t, int attributeQuark)
		throws StateSystemDisposedException;
	</pre>


- Query full state at a given timestamp

	<pre class="prettyprint" data-lang="java">
	List&lt;ITmfStateInterval&gt; queryFullState(long t)
		throws StateSystemDisposedException;
	</pre>

---
title: State Value Interface
subtitle: 

- All state values implement interface <code>ITmfStateValue</code>

	<pre class="prettyprint" data-lang="java">
	public enum Type {NULL, INTEGER, LONG, DOUBLE, STRING, CUSTOM;}
	</pre>

- Create a state value using state value factory <code>TmfStateValue</code>, for example:

	<pre class="prettyprint" data-lang="java">
	ITmfStateValue value = TmfStateValue.nullValue();
	ITmfStateValue intValue = TmfStateValue.newValueInt();
	ITmfStateValue longValue = TmfStateValue.newValueLong();
	</pre>	

- Read the value, for example: <code>IntegerStateValue</code>

	<pre class="prettyprint" data-lang="java">
	ITmfStateInterval interval = getInterval();
	if (interval.getValue().getType() == Type.Integer) {
		int retVal = interval.getValue().unboxInt();
	}
	</pre>

---
title: State Interval Interface
subtitle: 

- All state intervals implement interface <code>ITmfStateInterval</code>
- Has a start and end time

	<pre class="prettyprint" data-lang="java">
	long getStartTime();
	long getEndTime();
	</pre>
	
- Provides the quark and state value

	<pre class="prettyprint" data-lang="java">
	int getAttribute();
	ITmfStateValue getStateValue();
	</pre>

- Validates whether it intersects with given timestamp

	<pre class="prettyprint" data-lang="java">
	boolean intersects(long timestamp);
	</pre>

---
title: My other slide
subtitle: Subtitle Placeholder

- pressing 'f' toggle fullscreen
- pressing 'w' toggles widescreen
- pressing 'o' toggles overview mode
- pressing 'p' toggles speaker notes (if any)
- pressing 'h' highlights code snippets
- pressing 'b' toggles blank screen
- pressing 'c' toggles canvas to draw on the slide with the mouse
    - Pressing 'shift' draws arrow
    - Pressing 'alt' draws rectangle
    - Pressing 'shift+alt' draws ellipse
- pressing 'ESC' toggles of these goodies