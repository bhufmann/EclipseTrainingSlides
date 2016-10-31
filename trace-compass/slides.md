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
title: Project Explorer
subtitle: 

- Shows all available view under the analyses
- Trace needs to open to see available analyses
- Analysis requirements need to be fulfilled 

<center><img src="images/ProjectExplorerWithOutput.png" width="30%" height="30%"/></center>


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
	- Has a State Value
	- Changes over time
- Attribute Tree: 
	- Tree-like structure
	- Each attribute can have a value and sub-attributes
	- Access attributes using a path
- Quark:
	- Unique, constant identifier of an attribute
	- Makes faster queries

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
	  |     |- CURRENT_THREAD
	  |     |- SOFT_IRQS
	  |     |  |- &lt;Soft IRQ number&gt; -&gt; Soft IRQ Status
	  |     |- IRQS
	  |        |- &lt;IRQ number&gt; -&gt; IRQ Status
	  |- THREADS
	     |- &lt;Thread number&gt; -&gt; Thread Status
	        |- PPID
	        |- EXEC_NAME
	        |- PRIO
	        |- SYSTEM_CALL
	  </pre>

---

title: State System APIs
subtitle: 

- **TODO fix order**

- State value interface: <code>ITmfStateValue</code>
- State interval interface: <code>ITmfStateInterval</code>
- Read and write interface to the state system: <code>ITmfStateSystemBuilder</code>
- Query interface: <code>ITmfStateSystem</code>
- Analysis using a state system: <code>TmfStateSystemAnalysisModule</code>
- All state provider implement <code>ITmfStateProvider</code>
	- Extend abstract class <code>AbstractTmfStateProvider</code>
- Create a state system and assign a backend: <code>StateSystemFactory</code>

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

- Validates whether it intersects with a given timestamp

	<pre class="prettyprint" data-lang="java">
	boolean intersects(long timestamp);
	</pre>

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
title: State System Analysis Module
subtitle: TmfStateSystemAnalysisModule

- State system analysis modules typically extend <code>TmfStateSystemAnalysisModule</code>
- By default, full history on disk (default can be overwritten)
- Takes care of reading the trace using an event request
- Handles cancellation of analysis by user
- State system are stored in hidden directory **.tracing** in workspace
	- &lt;workspace&gt;/&lt;project&gt;/.tracing/&lt;trace&gt;/
	- State system file is: <code>&lt;analysis id&gt;.ht</code>
	- Delete state systems: Right-click on trace -> **Delete Supplementary Files...**

---
title: State System Analysis Module (2)
subtitle: TmfStateSystemAnalysisModule

- Access state system using utility method
	<pre class="prettyprint" data-lang="java">
	public static ITmfStateSystem getStateSystem(ITmfTrace trace, String moduleId);
	}
- Create state provider class 
<pre class="prettyprint" data-lang="java">
	protected ITmfStateProvider createStateProvider() {
		ITmfTrace trace = getTrace();
		if (trace == null) {
			throw new IllegalStateException();
		}
		return new ProcessingTimeStateProvider(trace);
	}
</pre>

---
title: State provider
subtitle: 

- All state provider implement interface <code>ITmfStateProvider</code>
- Typically, extend <code>AbstractTmfStateProvider</code>
	- Uses a buffering scheme to not block event request
- Implement <code>ITmfStateProvider</code>#getVersion() 
	- To force recreation of state system change return value
- Implement <code>ITmfStateProvider</code>#getInstance()
- Implement <code>AbstractTmfStateProvider</code>#eventHandle()

---
title: State provider (2)
subtitle:
content_class: smaller 

<pre class="prettyprint" data-lang="java" >
	protected void eventHandle(@NonNull ITmfEvent event) {
		final ITmfStateSystemBuilder stateSystem = getStateSystemBuilder();
		switch (event.getName()) {
		case IEventConstants.CREATE_EVENT:
			// get event field with name
			String requester = event.getContent().getFieldValue(String.class, "requester");

			// get quark of attribute for path Requester/&lt;requester&gt;
			int quark = stateSystem.getQuarkAbsoluteAndAdd("Requester", requester);

			// Create new state value
			ITmfStateValue stateValue = TmfStateValue.newValueInt
				(IEventConstants.ProcessingStates.INITIALIZING.ordinal());

			// get time of event
			long t = event.getTimestamp().getValue();

			// apply state change
			stateSystem.modifyAttribute(t, stateValue, quark);
			return;
		}
	}
</pre>

---
title: Exercise: Implement a state provider
subtitle: 

- Reset to **TRACE_COMPASS_???**
- Open <code>ProcessingTimeAnalysis</code>
	- Implement createStateProvider()
	- Return an instance of <code>ProcessingTimeStateProvider</code> (class already exists) 
- Implement <code>ProcessingTimeStateProvider</code>
	- See next slides for state machine and attribute tree  
- Run Trace Compass and open trace
- Open State System Explorer (Window -> Show View -> Tracing -> State System Explorer)
	- Explore state system org.eclipse.tracecompass.training.example.ht 
	- Navigate events table and follow state changes
		- Hint: add search criteria (:CREATE|:START|:STOP|:END)
- **Go!**

---
title: Exercise State Machine
subtitle: 

<center><img src="images/ExerciseStateMachine.png" width="50%" height="50%"/></center>

- Note: When receiving **ust_master:end** set the state to the null state!

---
title: Exercise Attribute Tree
subtitle:

 
	<pre>
	  |- Requester
	        |- &lt;requester&gt; -&gt; State Value
	  </pre>

- Example path: Requester/&lt;requester&gt;
	- Where &lt;requester&gt; is taken from event field of CTF event
- State values:
	- 0=INITIALIZING
	- 1=PROCESSING
	- 2=WAITING

---

title: Bonus exercise: 2nd State Machine
subtitle: 

- Reset to **TRACE_COMPASS_???**
- Update <code>ProcessingTimeStateProvider</code> (see state machine on next slide)
	- Hint: Use attribute tree layout shown in file 
- Run Trace Compass and open trace
- Open State System Explorer
	- Explore state system org.eclipse.tracecompass.training.example.ht 
	- Navigate events table and follow state changes
- **Go!**

---

title: Example State Machine (2)
subtitle: 

<center><img src="images/ExerciseProcessingStateMachine.png" width="45%" height="45%"/></center>

---
title: Exercise Attribute Tree
subtitle:

 
	<pre>
	  |- Requester
	        |- &lt;requester&gt; -&gt; State Value
	              |-&lt;id&gt;   -&gt; State Value
	  </pre>

- Example path: Requester/&lt;requester&gt;/&lt;id&gt;
	- Where &lt;requester&gt; is taken from event field of CTF event
- State values:
	- 0=INITIALIZING
	- 1=PROCESSING


---
title: Exercise: Review
subtitle: 

- Overview of Generic State System APIs (for building)
- Creating a state system analysis module
- Implementing a state system provider
- Exploring of a state system using the State System Explorer
- Deleting the supplementary files

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

	<pre class="prettyprint" data-lang="java">
    int optQuarkAbsolute(String... attribute)
            throws AttributeNotFoundException;
	</pre>

- Getting a quark of an optional attribute from relative path

	<pre class="prettyprint" data-lang="java">
	int optQuarkRelative(int startingNodeQuark, String... subPath)
		throws AttributeNotFoundException;
	</pre>
- Return <code>#INVALID_ATTRIBUTE</code> (-2) if it doesn't exist

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
title: Query a state system (5)
subtitle: StateSystemUtils

- Utility class to query history range
- Get all the states for given quark between start and end time

	<pre class="prettyprint" data-lang="java">
	public static List<ITmfStateInterval> queryHistoryRange(ITmfStateSystem ss, int attributeQuark, 
		long t1, long t2)
			throws AttributeNotFoundException, StateSystemDisposedException
	</pre>

- Get all the states for given quark between start and end time with resolution

	<pre class="prettyprint" data-lang="java">
	public static List<ITmfStateInterval> queryHistoryRange(ITmfStateSystem ss, int attributeQuark, 
		long t1, long t2, long resolution, IProgressMonitor monitor)
			throws AttributeNotFoundException, StateSystemDisposedException
	</pre>

---
title: Exercise: Query a state system
subtitle: 

- Reset to **TRACE_COMPASS_???**
- Open view class ProcessingStatesView
	- Implement TODOs to query sate system in method print states
	- Use <code>ITmfStateSystem</code> interface and utility <code>StateSystemUtils</code> 
	- Use method outputText() to print text on screen
- **Go!**	

---
title: Exercise: Review
subtitle: 

- State values and state intervals
- Getting attribute quarks
- Performing  single queries
- Performing full queries
- Querying a history range

---
title: Module 5
subtitle: Time Graph views

- Time Graph Viewer and its Components
- Time Graph Viewer Model
- Time Graph view details
- Filtering in Time Graph view
- Searching in Time Graph view
- Sorting in Time Graph view

---
title: Overview
subtitle: 

- Visualizes states over time
	- For example, processes, threads, cores, IRQs...
- Works well with state systems
- Uses common widget library on top of SWT
- Extensible for custom use cases


---
title: Time Graph view components
subtitle: 

- 

<center><img src="images/TimeGraphView-explained.png" width="100%" height="100%"/></center>


---
title: Time Graph viewer Overview
subtitle:

- Visualizes states over time
	- For example, processes, threads, cores, IRQs...
- Common widget library on top of SWT
- Provides common features
	- Navigation with mouse, keyboard and toolbar buttons
	- Zoom-in and out
	- Highlight of regions as marker overlays (e.g. Bookmarks)
	- Time cursors
	- Searching (rows!)
- Supports arrows between events
- Tree structure that supports columns

---
title: Time Graph viewer
subtitle: 

- Create a Time Graph viewer instance <code>TimeGraphViewer</code>
- Define content provider to provide Time Graph Model root entries
- Define a presentation provider to define how to display states
- Define a filter content provider for filter dialog

	<pre class="prettyprint" data-lang="java">
	TimeGraphViewer viewer = new TimeGraphViewer();
	viewer.setContentProvider(new MyTimeGraphContentProvider());
	viewer.setPresentationProvider(new MyPresentationProvider);
	viewer.setFilterContentProvider();
	viewer.setInput(getModel();
	</pre>


---
title: Time Graph Model
subtitle: 

<center><img src="images/TimeGraphView-Model.png" width="50%" height="50%"/></center>

---

title: Time Graph Model (2)
subtitle: ITimeGraphEntry

<center><img src="images/TimeGraphEntries.png" width="70%" height="70%"/></center>

- All time graph models implement interface <code>ITimeGraphEntry</code>
- 	Typically models extend default implementation <code>TimeGraphEntry</code>
- It's a tree structure: ITimeGraphEntry has 0..* <code>ITmfGraphEntry</code> children
- Use content provider the root entries can be supplied for a model object

	<pre class="prettyprint" data-lang="java">
	ITimeGraphEntry getParent();
	List&lt;ITimeGraphEntry&gt; getChildren();
	String getName();
	boolean hasTimeEvents();
	Iterator&lt;? extends ITimeEvent&lt; getTimeEventsIterator();
	</pre>


---
title: Time Graph Model (3)
subtitle: ITimeEvent

<center><img src="images/TimeEvents.png" width="50%" height="50%"/></center>

- Each <code>ITimeGraphEntry</code> has 0..* more time events
- Time events define the state intervals to be displayed
- All time events implement interface <code>ITimeEvent</code>
- Typically time events extend default implementation <code>TimeEvent</code>

	<pre class="prettyprint" data-lang="java">
	ITimeGraphEntry getEntry();
	long getTime();
	long getDuration();
	</pre>


---
title: Presentation Provider
subtitle: 

- Provide the colors to be used for each time event
- Define the tooltip to show when hovering over a time event
- Provide possibility to draw overlays over an time graph entry, time event or control
- Customize the entry height 
- All presentation provider implement interface <code>ITimeGraphPresentationProvider</code>
- Typically presentation provider extend <code>TimeGraphPresentationProvider</code>

	<pre class="prettyprint" data-lang="java">
	StateItem[] getStateTable();
	int getStateTableIndex(ITimeEvent event);
	void postDrawEvent(ITimeEvent event, Rectangle bounds, GC gc);
	Map<String, String> getEventHoverToolTipInfo(ITimeEvent event);
	// ...
	</pre>

---
title: TODO marker axis
subtitle: 


---
title: Exercise: Create a Time Graph Viewer
subtitle: 

- Reset to **TRACE_COMPASS_???**
- Open view class ProcessingStatesView
- In createPartControl()
	- create new instance of TimeGraphViewer
	- set presentation provider (<code>ProcessingStatesPresentationProvider</code>)
- Implement time graph model in method fillTimeGraph() see TODOs
- Run Trace Compass and explore the Time Graph Viewer features
- Question: What limitations do you foresee with this implementation?
- **Go!**

---
title: Exercise: Review
subtitle: 

- Creating a Time Graph Viewer
- Setting a presentation provider
- Creating a time graph model from a state system
- Exploring of the Time Graph Viewer navigation

---
title: Time Graph View
subtitle: 

- Eclipse view wrapping Time Graph Viewer 
- Common abstract class with re-occurring and re-usable code
	- Listens to TMF signals (e.g. trace opened, time range selected)
	- Loads the view content
	- Provides default set of buttons
	- Common synchronized time axis
- Provides support for lazy loading of viewer
- Provides marker list
- Provides list of linked time events
- Interfaces state systems

---
title: Time Graph View (2)
subtitle: 

- Typically views extend
	- <code>AbstractTimeGraphView</code>
	- <code>AbstractStateSystemTimeGraphView</code>
- Use <code>AbstractTimeGraphView</code> to populate each row at a time
	- Small number of rows
	- Works with or without state systems
- Use <code>AbstractStateSystemTimeGraphView</code> to populate per pixel column (all entries)
	- High number of rows
	- Uses full state system queries
	- Works only with state systems

---
title: Time Graph View (3)
subtitle: 

<center><img src="images/AbstractTimeGraphView.png" width="80%" height="80%"/></center>

---
title: Time Graph View Concepts
subtitle: AbstractTimeGraphView

- Build thread
	- Builds a list of time graph entries (rows)
	
	<pre class="prettyprint" data-lang="java">
	protected abstract void buildEntryList(ITmfTrace trace, ITmfTrace parentTrace, IProgressMonitor monitor);
	</pre>
	
- Zoom thread
	- Builds time event list per zoom level and display resolution

	<pre class="prettyprint" data-lang="java">
	protected abstract List&lt;ITimeEvent&gt; getEventList(TimeGraphEntry entry,	long startTime, long endTime, long resolution, IProgressMonitor monitor);
	</pre>

---
title: Time Graph View Concepts
subtitle: AbstractStateSystemTimeGraphView

- Build thread

	<pre class="prettyprint" data-lang="java">
	protected abstract void buildEntryList(ITmfTrace trace, ITmfTrace parentTrace, IProgressMonitor monitor);
	</pre>

- Call queryStateSystem in buildEntryList() and provide call back <code>IQueryHandler</code>

	<pre class="prettyprint" data-lang="java">
	protected void queryFullStates(ITmfStateSystem ss, long start, long end, 
		long resolution, IProgressMonitor monitor, IQueryHandler handler)
	</pre>

- Zoom thread

	<pre class="prettyprint" data-lang="java">
	protected abstract List<ITimeEvent> getEventList(TimeGraphEntry tgentry, ITmfStateSystem ss, 
		List&lt;List&lt;ITmfStateInterval&gt;&gt; fullStates, List&lt;ITmfStateInterval&gt; prevFullState, 
		IProgressMonitor monitor);
	</pre>


---
title: Exercise: Create a Time Graph Viewer
subtitle: 

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