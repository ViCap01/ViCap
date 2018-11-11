# ViCap
## New Task: Video Interactive Captioning with Human Prompts (ViCap)

As a video clip often includes rich content and semantic details, different people may be interested in different views. Thus a caption generated by an existing video captioning method always fails to meet the ad hoc expectations. We propose a new task called **Video Interactive Captioning (ViCap)**, i.e., when the initial caption generated by a captioning agent does not meet human expectation, we could launch a round of interaction between a human and the agent and give a short prompt to the agent. Then the agent could generate a more accurate caption based on the prompt. As shown below, we show an example. A human and a ViCap agent are interacting. First, the agent generates an initial description of a video clip (the blue words). As the human may be interested in certain frames of the clip, the description fails to meet his expectation. Then he gives a short prompt, i.e., the first two words of the expecting caption (the purple words). Finally, based on the prompt, the agent generates a more accurate caption (the red words).
![Task](https://github.com/ViCap01/ViCap/blob/master/pic/intro.jpg "An example of our task")
