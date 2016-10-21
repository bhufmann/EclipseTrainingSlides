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
content_class: smaller

- Overview
- Analysis Module
- Plug-in extension point
- Analysis Requirements
- Analysis Dependency
- Analysis Exercise 1 

---

title: Analysis Framework Overview
subtitle:
content_class: smaller

- API for integrating trace analyses
- Plug-in extension point
- Shows what can be done with trace content
- Provides hooks to add views
- Executed automatically or on demand
- Manages dependencies between multiple analyses
- Manages requirements to execute analysis
- Can have parameters
- Runs a separated thread


TODO: picture

---
title: Analysis Module
subtitle:
content_class: smaller

- API for data collection
- All analyses implement <code>ITmfAnalysisModule</code>
- Abstract implementation <code>TmfAbstractAnalysisModule</code>
- 0..N analyses per trace or experiment

    <pre class="prettyprint" data-lang="java">
    public class ProcessingTimeAnalysis extends TmfAbstractAnalysisModule {
        public ProcessingTimeAnalysis() {
        }
        @Override
        protected boolean executeAnalysis(IProgressMonitor monitor) throws TmfAnalysisException {
            // TODO Auto-generated method stub
            return true;
        }
        @Override
        protected void canceling() {
            // TODO Auto-generated method stub
        }
    }
    </pre>

---
title: Plug-in Extension Point
subtitle:
content_class: smaller

- Identifier: org.eclipse.linuxtools.tmf.core.analysis

    <pre class="prettyprint" data-lang="xsd">
     <!ELEMENT module (parameter , tracetype)*>
     <!ATTLIST module
     id                 CDATA #REQUIRED
     name               CDATA #REQUIRED
     analysis_module    CDATA #REQUIRED
     icon               CDATA #IMPLIED
     automatic          (true | false)
     applies_experiment (true | false) >
    </pre>

- id - The unique ID that identifies this analysis module.
- name - The trace analysis module's name as it is displayed to the end user
- analysis_module - The fully qualified name of a class that implements the IAnalysisModule interface.
- icon - The icon associated to this analysis module.
- automatic - Whether to execute this analysis automatically when trace is opened, or wait for the user to ask for it
- applies_experiment - If it applies to traces or experiments.

---
title: Plug-in Extension Point (2)
subtitle:
content_class: smaller

- Define the tracetype the analysis applies (or not)

    <pre class="prettyprint" data-lang="xsd">
     <!ELEMENT tracetype EMPTY&>
     <!ATTLIST tracetype
     class   CDATA #REQUIRED
     applies (true | false) >
    </pre>
- class - The base trace class this analysis applies to or not (it also applies to traces extending this class).
- applies - Does this tracetype element mean the class applies or not (default true)

---
title: My other slide
subtitle: Subtitle Placeholder
content_class: smaller

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