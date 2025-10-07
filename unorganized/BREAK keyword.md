#4. Starting a New Chunk
The BREAK keyword (must be uppercase) is used to fill the current chunks with padding characters. When you add BREAK and then continue with more text, it starts a new chunk. This helps to ensure that the following prompts are not influenced by the previous text. Let's use an example to better understand how this works.

Left image: 1girl, sitting on a bench in a park, high resolution, sunlight, smiling, head resting on hands, long hair, round glasses, pink eyes, white dress, purple hair, green headband.

Right image: 1girl, sitting on a bench in a park, high resolution, sunlight, smiling, head resting on hands, long hair, round glasses, pink eyes, white dress, purple hair, BREAK, green headband.



https://www.aiarty.com/stable-diffusion-prompts/stable-diffusion-prompt-guide.htm

---





https://www.reddit.com/r/StableDiffusion/comments/15bty86/prompt_trick_for_more_consistent_results_in/



This is very simple and has been a built-in function in Auto1111 for a long time, but I didn't know it made such a difference for getting consistent results. I just want to share it in case it can help others:



In Auto1111, SD processes the prompts in chunks of 75 tokens. We all know that prompt order matters - what you put at the beginning of a prompt is given more attention by the AI than what goes at the end. But here's the thing: This rule isn't about the *whole* prompt, but for each *chunk*. The AI gives more attention to what comes first in each *chunk*. So if you have a very long prompt of 300 tokens or so, the attention will be highest on the first few tokens, then token 76-80, then 151-155, then again at 226-230 etc. Every 75 tokens, you get a peak of attention. Just a minor change in the order of your prompt around these points will matter a whole lot, but at other spots in your prompt the order will make very little difference.



Here is how to use it. Say you have five things you want the AI to pay special attention to... Then you use the command BREAK to start a new chunk even before you get to a multiple of 75 tokens, and then you put the important concepts first after each BREAK, like this:

---

**analog style, Nikon Z 85mm camera RAW**, (best quality:1.2), (masterpiece:1.2), award winning glamour photograph, (realistic:1.2), (intricately detailed:1.1)

BREAK **1girl, (looking at viewer:1.2)**, beautiful woman (galgadot22:1.1) as Chinese courtesan, athletic lean body, perfect face, beautiful eyes

BREAK **wearing a silky (qipao:1.2)** traditional Chinese cheongsam dress with (short:1.2) skirt, slit slitted skirt, in a traditional Chinese rural home

BREAK **by camille souter, saturated colors**, cinematic, warm dramatic sidelight, bloom, bokeh, blurry background, depth-of-field

BREAK **looking at viewer, subtle smile**, (perfect hands:0.8), erotic, seductive, cute

<lora:galgadot22:0.76> <lora:qipao_lora:0.7>

---

Now I know *exactly* which words the AI will pay most attention to - the ones I wrote in bold here - because they're at the beginning of each chunk. If I had just typed this prompt as one long string without the breaks, the attention would not have been sure to be on those words.

Does the prompt work without the breaks? Sure it does - but you get much less predictable results, and any minor change to your prompt could cause a completely new word to become randomly more important.

I really wish someone had explained this trick to me earlier, but now that I learned about it I'm sharing it. Hope it helps you get more consistent results.
