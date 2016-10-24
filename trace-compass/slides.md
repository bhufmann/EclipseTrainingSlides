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
content_class: bigger

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
content_class: bigger

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
content_class: bigger

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
content_class: bigger

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
content_class: bigger

- Click on **Add...** Button
- Find org.eclipse.linuxtools.tmf.core.analysis
- Right click on org.eclipse.linuxtools.tmf.core.analysis -> New -> module
- Fill-in relevant data (id, name analysis_module)

<center><img src="images/ExtensionAnalysisModule.png" width="80%" height="80%"/></center>

---
title: Project Explorer
subtitle: 
content_class: bigger

- Shows all available analyses under trace or experiment
- Note: Need to open trace to see available analyses

<center><img src="images/ProjectExplorerWithAnalysis.png" width="40%" height="40%"/></center>

---
title: Exercise: Create an analysis
subtitle: 
content_class: bigger

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
content_class: bigger

- Defining an analysis extension
- Implementing an analysis module class
- Running the analysis
- Exploring the integration in the Project Explorer

---

title: Apply to Trace Type
subtitle: 
content_class: bigger

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
content_class: bigger

- Right-click on analysis module -> New -> tracetype
- Fill-in the class

<center><img src="images/ExtensionAnalysisModule-TraceType.png" width="80%" height="80%"/></center>

---

title: Exercise: Apply to trace type
subtitle: 
content_class: bigger

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
content_class: bigger

- Applying analysis to a trace type
- Exploring the analysis in Project Explorer
	- Analysis shown for corresponding trace type
	- Analysis not shown for other trace type 
- Exploring the integration in the Project Explorer

---
title: Analysis Requirements
subtitle: 
content_class: bigger

- 
- Provide information to user if analysis can't run
- Requirements on event types or specific event field
- Implement interface <code>IAnalysisRequirementProvider</code>

	<pre class="prettyprint" data-lang="java">
	public interface IAnalysisRequirementProvider {
		Iterable<TmfAbstractAnalysisRequirement> getAnalysisRequirements();
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
content_class: bigger

- 

	<pre class="prettyprint" data-lang="java">
	@Override
	public Iterable<TmfAbstractAnalysisRequirement> getAnalysisRequirements() {
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
content_class: bigger

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
content_class: bigger

- Implementing analysis requirement for event names
- Providing the analysis requirement from the analysis 
- Exploring the analysis in Project Explorer
	- Analysis shown if all requirements are fulfilled
	- Otherwise analysis is striked-through
- Showing the analysis help  

---

title: Analysis Parameter Provider
subtitle:
content_class: bigger

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
content_class: bigger

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
content_class: bigger

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
content_class: bigger

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
content_class: bigger

- Right-click on <code>org.eclipse.linuxtools.tmf.core.analysis</code> -> New -> output
	- Fill in class of output and id of view
- Right-click on output -> New -> analysisModuleClass
	- Fill-in id of analysis

<center><img src="images/AnalysisOutputExtension.png" width="70%" height="70%"/></center>

---
title: Exercise: Create an output
subtitle: 
content_class: bigger

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
content_class: bigger

- Implementing analysis output
- Opening the output from the Project Explorer
- Exploring the analysis in Project Explorer

---

title: My other slide
subtitle: Subtitle Placeholder
content_class: bigger

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