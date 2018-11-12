# ViCap
## New Task: Video Interactive Captioning with Human Prompts (ViCap)

As a video clip often includes rich content and semantic details, different people may be interested in different views. Thus a caption generated by an existing video captioning method always fails to meet the ad hoc expectations. We propose a new task called **Video Interactive Captioning (ViCap)**, i.e., when the initial caption generated by a captioning agent does not meet human expectation, we could launch a round of interaction between a human and the agent and give a short prompt to the agent. Then the agent could generate a more accurate caption based on the prompt. As shown below, we show an example. A human and a ViCap agent are interacting. First, the agent generates an initial description of a video clip (the blue words). As the human may be interested in certain frames of the clip, the description fails to meet his expectation. Then he gives a short prompt, i.e., the first two words of the expecting caption (the purple words). Finally, based on the prompt, the agent generates a more accurate caption (the red words).
![Task](https://github.com/ViCap01/ViCap/blob/master/pic/intro.jpg "An example of our task")

From a practical view, a human could provide any prompts in any form, e.g., in different length. And the prompts could appear anywhere in the generated captions. In this paper, we move forward a first step in that we set the prompts to be the first two words of the generating caption. Thus this is a task which is different from traditional captioning tasks, which may support various applications, such as video retrieval, video chat robot, and even human-robot interaction. Here, we devise the ViCap to include a video encoder, an initial caption encoder, and a refined caption generator. Moreover, we show that the ViCap can be trained via a full supervision way, i.e., supervised with the ground-truth captions, or a weak supervision way, i.e., weakly supervised with only the prompts.

## The Decoder of ViCap Model

We use a GRU unit to encode the video and the initial caption. Then we devise a improved GRU decoder and a convolutional decoder. Particularly, for the convolutional decoder, we stack multiple dilated convolutional layers followed by gated activation units, the goal of which is to capture dependencies among frame and word sequences. As shown below, we illustrate the structure of the convolutional decoder which consists of five dilated convolutional layers. The dilated rate is respectively set to 1, 1, 2, 4, 2. The green words are the ones generated by the decoder. By the hierarchical convolutional architecture, the decoder could capture variant dependencies among the sequence.
![Task](https://github.com/ViCap01/ViCap/blob/master/pic/cnndecoder.jpg "Illustration of CNN decoder")
