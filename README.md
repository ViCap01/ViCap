# ViCap
## New Task: Video Interactive Captioning with Human Prompts (ViCap)

As a video clip often includes rich content and semantic details, different people may be interested in different views. Thus a caption generated by an existing video captioning method always fails to meet the ad hoc expectations. We propose a new task called **Video Interactive Captioning (ViCap)**, i.e., when the initial caption generated by a captioning agent does not meet human expectation, we could launch a round of interaction between a human and the agent and give a short prompt to the agent. Then the agent could generate a more accurate caption based on the prompt. As shown below, we show an example. A human and a ViCap agent are interacting. First, the agent generates an initial description of a video clip (the blue words). As the human may be interested in certain frames of the clip, the description fails to meet his expectation. Then he gives a short prompt, i.e., the first two words of the expecting caption (the purple words). Finally, based on the prompt, the agent generates a more accurate caption (the red words).
![Task](https://github.com/ViCap01/ViCap/blob/master/pic/intro.jpg "An example of our task")

From a practical view, a human could provide any prompts in any form, e.g., in different length. And the prompts could appear anywhere in the generated captions. In this paper, we move forward a first step in that we set the prompts to be the first two words of the generating caption. Thus this is a task which is different from traditional captioning tasks, which may support various applications, such as video retrieval, video chat robot, and even human-robot interaction. Here, we devise the ViCap to include a video encoder, an initial caption encoder, and a refined caption generator. Moreover, we show that the ViCap can be trained via a full supervision way, i.e., supervised with the ground-truth captions, or a weak supervision way, i.e., weakly supervised with only the prompts.

## The Decoder of ViCap Model

We use a GRU unit to encode the video and the initial caption. Then we devise a improved GRU decoder and a convolutional decoder. Particularly, for the convolutional decoder, we stack multiple dilated convolutional layers followed by gated activation units, the goal of which is to capture dependencies among frame and word sequences. As shown below, we illustrate the structure of the convolutional decoder which consists of five dilated convolutional layers. The dilated rate is respectively set to 1, 1, 2, 4, 2. The green words are the ones generated by the decoder. By the hierarchical convolutional architecture, the decoder could capture variant dependencies among the sequence.
![Task](https://github.com/ViCap01/ViCap/blob/master/pic/cnndecoder.jpg "Illustration of CNN decoder")

## ViCap with Prompt Supervision

As shown below, we take the GRU decoder as an example to illustrate the difference between these two training ways, i.e., during the training of full supervision (the left part), at each time step, we take the ground-truth word (the red word) as the input of decoder to predict the next word. Whereas, during the training of prompt supervision (the right part), besides the prompt words (the purple words), we only take the previous output (the green word) of the decoder as the current input of the decoder.
![Task](https://github.com/ViCap01/ViCap/blob/master/pic/prompts.jpg "Illustration of prompt supervision")

For the weak supervision with only prompts, we devise a method which use the output of the decoder to reconstruct the input of the decoder. As shown below, we illustrate the structure of GRU reconstruction network.
![Task](https://github.com/ViCap01/ViCap/blob/master/pic/GRURecon.jpg "Illustration of GRU Reconstruction Network")

As shown below, we illustrate the structure of convolutional reconstruction network. And we stack three dilated convolutional layers as the reconstruction network. The dilated rate is respectively set to 1, 2, 2.
![Task](https://github.com/ViCap01/ViCap/blob/master/pic/cnnrecon.jpg "Illustration of Convolutional Reconstruction Network")

## Dataset

We extend MSRVTT-2016 dataset. And we take the 1st to 4500th video clips of the dataset as the training set of the pre-trained model and use the 4501st to 5000th video clips of the dataset as the validation set. For ViCap models, we take the 5001st to 8500th video clips of the dataset as the training set. And we take the 8501st to 9000th video clips as the validation set and use 9001st to 10000th video clips as the test set. Besides, for each clip of the validation and test set of ViCap models, we give three different semantic sentences as the annotations. Finally, when we evaluate the performance of ViCap models, for each one of the annotations in the test set, we remove the first two words and take the remaining words as the ground-truth of evaluation.

**Examples**: `Videos we choose to show here are short limited the the size constraint of GIF in github.`

| Videos | Interactive Captioning pairs |
| ---- | ---- |
|![example1](https://github.com/ViCap01/ViCap/blob/master/pic/example1.gif "Example1") | **Initial**: ["a man is talking about a car."]  **Prompts0**: ["a kids."]  **Ground-truth0**: ["a kids wearing a hat going inside the bus and two persons are standing behind them."] **Prompts1**: ["a man."] **Ground-truth1**: ["a man along with his two kids going inside the bus."] **Prompts2**: ["westlake fire."] **Ground-truth2**: ["westlake fire department men are save and rescue in small children."]  |
|![example2](https://github.com/ViCap01/ViCap/blob/master/pic/example2.gif "Example2") | **Initial**: ["a woman is talking."]  **Prompts0**: ["woman in."]  **Ground-truth0**: ["woman in athletic clothing carry their babies during a workout."] **Prompts1**: ["a woman."] **Ground-truth1**: ["a woman is sitting with her baby and two people talking about that."] **Prompts2**: ["lady holds."] **Ground-truth2**: ["lady holds a baby and two people analyze the events."]  |
|![example3](https://github.com/ViCap01/ViCap/blob/master/pic/example3.gif "Example3") | **Initial**: ["a woman is talking."]  **Prompts0**: ["two girls."]  **Ground-truth0**: ["two girls wearing wollen coat and having the mike in hands and talk about the fashion."] **Prompts1**: ["a woman."] **Ground-truth1**: ["a woman in pink sweater and black cap is talking to another woman in white cap."] **Prompts2**: ["a girl."] **Ground-truth2**: ["a girl in white dressing is talking to a woman."]  |
|![example4](https://github.com/ViCap01/ViCap/blob/master/pic/example4.gif "Example4") | **Initial**: ["a man is talking to a crowd."]  **Prompts0**: ["a group."]  **Ground-truth0**: ["a group of people are talking at a wwe event."] **Prompts1**: ["a man."] **Ground-truth1**: ["a man is talking to a man about wwe."] **Prompts2**: ["an announcer."] **Ground-truth2**: ["an announcer speaks into a microphone at a wwe match."]  |
|![example5](https://github.com/ViCap01/ViCap/blob/master/pic/example5.gif "Example5") | **Initial**: ["a man is driving a car in a road."]  **Prompts0**: ["a people."]  **Ground-truth0**: ["a people is riding a ski lift type of vehicle up the side of the hill."] **Prompts1**: ["people ride."] **Ground-truth1**: ["people ride a gondola up a steep mountain and pass picturesque views."] **Prompts2**: ["a beautiful."] **Ground-truth2**: ["a beautiful view of greenery and river from a top view."]  |
|![example6](https://github.com/ViCap01/ViCap/blob/master/pic/example6.gif "Example6") | **Initial**: ["a woman is talking about a woman."]  **Prompts0**: ["a girl."]  **Ground-truth0**: ["a girl casts a spell to alter another girls hair."] **Prompts1**: ["disney is."] **Ground-truth1**: ["disney is promoting a new show for their channel."] **Prompts2**: ["some scenes."] **Ground-truth2**: ["some scenes from a tv show are playing in which people are talking."]  |
