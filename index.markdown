---
layout: default
---

{: .text-left}
<span class="sys-name">ExpressEdit</span> is a system that enables editing videos via NL text and sketching on the video frame. Powered by LLM and vision models, the system interprets **(1) temporal, (2) spatial, and (3) operational references in an NL command and spatial references from sketching**. The system implements the interpreted edits, which then the user can iterate on.
<br/><br/>
<span class="sys-name">ExpressEdit</span>'s design is motivated by an analysis of **176 multimodal expressions of editing commands** from 10 video editors, which revealed the patterns of use of NL and sketching in describing edit intents.
<br/><br/>
We present two iterations of <span class="sys-name">ExpressEdit</span>'s interface and the overview of the pipeline.

{: .img-right}
![Animation of the overview of the Computer Vision & LLM-based pipeline.](/assets/img/animation-v5.gif)

------

## System Interface (Iteration 1)

{: .sys-img}
![Main interface of ExpressEdit](/assets/img/old-system.png)

The first iteration of <span class="sys-name">ExpressEdit</span>'s interface is designed to allow users to (1) express their edit commands through natural language text and sketching on the video and (2) iterate on their edit commands as well as manually manipulate the interpretation results.


<br/>

{: .text-left}
<span class="sys-name">ExpressEdit</span> supports **Edit List** that allows the user to add a layer on top of the video that contains a set of edits. The user can press the *Add* button to add a layer and the edit commands issued by the user will be stored under the same layer. 
<br/><br/>
To describe the edit command, the user can use the **Edit Description** panel and specify *Natural Language Text* or *Sketch* on top of the frame. 

{: .img-right}
![The animation that shows initiation of the edit command](/assets/img/old-initiating.gif)

<span class="sys-name">ExpressEdit</span> processes the edit command and automatically suggests several candidates at several moments in the video. It automatically specifies the edit parameters & the location of each edit.


{: .img-left}
![The animation that shows the suggested edits and the reasoning](/assets/img/old-examining.gif)

{: .text-right}
<br/><br/>
<span class="sys-name">ExpressEdit</span> allows users evaluate if the suggestions are appropriate through **Examine** panel that provides the reasoning for the suggestion. It shows the reasoning for the temporal & spatial location of the edit as well as the breakdown of the NL edit command.
<br/><br/>
The user can go over the suggestions and either *accept* them or *reject* them. The user can manipulate each applied edit manually too.

------

## System Interface (Iteration 2)

{: .sys-img}
![Main interface of ExpressEdit](/assets/img/new_system.jpg)

The second iteration of <span class="sys-name">ExpressEdit</span>'s interface is based on findings from the observational study (N=10) conducted with the first version of the interface.  


<br/>

{: .text-left}
Similar to the first version, <span class="sys-name">ExpressEdit</span> supports **Tabs** that allow the user to add a layer on top of the video by pressing **+**. However, now, the user can see the edit commands they issued as well as the suggested edits in the scrollable panel.

{: .img-right}
![The animation that shows initiation of the edit command](/assets/img/new-initiating.gif)

<span class="sys-name">ExpressEdit</span> returns three types of responses after processing the user’s edit command:

![Three types of panels that ExpressEdit returns](/assets/img/new-panels.png)

(a) The **Examine** panel analyzes the user’s natural language command and shows which parts of the input correspond to the description of temporal location, spatial location, and edit operation type and parameters.

(b) The **Edit result** panel shows the preview of the resulting edit by providing a snapshot of the edit together with explanations on why the segment was selected for the edit to apply.

(c) The **Summary** panel allows the user to select edits the user wants to apply among generated edits. As well as *request more edits*, *move the set of edits to a new tab*, or *navigate to the previous summary*


{: .img-left}
![The animation that shows the suggested edits and the reasoning](/assets/img/new-examining.gif)

{: .text-right}
Users can browse the suggested edits and turn them on/off as they edit the video.

------

## Pipeline

<span class="sys-name">ExpressEdit</span> is powered by CV & LLM based pipeline that interprets NL & Sketch edit commands. It consists of 2 stages: (1) the offline stage which preprocesses the footage to extract useful metadata; (2) the online stage which interprets the edit commands of the user using the preprocessed metadata.

### Preprocessing stage (offline)

![The animation that shows the preprocessing stage of the pipeline](/assets/img/pipeline-offline.gif)

The pipeline first preprocesses the footage video and extracts textual descriptions of *10-second segments* and segmentations of each *1-second frame* of the video.

Textual descriptions consist of (1) **synthetic caption** & **salient objects** by <a href="https://huggingface.co/docs/transformers/main/model_doc/blip-2">BLIP-2</a>, (2) **recognized actions** by <a href="https://github.com/OpenGVLab/InternVideo">InternVideo</a>, and (3) **transcript** from original Youtube video by  <a href="https://github.com/yt-dlp/yt-dlp">yt-dlp</a>.

The pipeline also extracts **segmentations** using <a href="https://github.com/facebookresearch/segment-anything">Segment-Anything</a> excluding the areas that are small.

### Interpretation stage (online)

![The animation that shows how the pipeline handles the online requests](/assets/img/pipeline-online.gif)

As the user issues an edit command, the pipeline parses the NL command and interprets each of the references (temporal, spatial, and edit) by using the preprocessed metadata with corresponding <a href="https://openai.com/gpt-4">GPT-4</a> prompts.

In particular, we use both <a href="https://openai.com/gpt-4">GPT-4</a> and <a href="https://github.com/openai/CLIP">CLIP</a> to interpret the spatial location as the pipeline may need to consider the user's sketch and the segmentations from the metadata.

------

## Bibtex
<pre>

TBA

</pre>

------

{: .logos}
[![Logo of KIXLAB](/assets/img/kixlab_logo.png)](https://kixlab.org)
[![Logo of KAIST](/assets/img/kaist_logo.png)](https://kaist.ac.kr)

{: .center .acknowledgement}
This work was supported by the National Research Foundation of Korea (NRF) grant funded by the Korean government (MSIT) (NRF-2020R1C1C1007587) and the Institute of Information & Communications Technology Planning & Evaluation (IITP) grant funded by the Korean government (MSIT) (No.2021-0-01347, Video Interaction Technologies Using Object-Oriented Video Modeling).