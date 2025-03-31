<!-- ![](images/shadow.jpg)  -->
<!-- ![](images/divider.jpg) -->

*Updated 31 March, 2025 · Toulouse*

![](images/divider.jpg)

### Prompt from a File

This (for me) turned out to be a dead end:

- the file has to be uploaded (I was hoping to use a file on disk that is read each time while SD continues to operate in the background)
- it does not seem to be able to handle ADetailer or upscaling.

![](images/divider.jpg)

The prompts in the file will be added at the **start** or **end** of the prompts entered in the main prompt text field (so empty the main text field if you want prompts to come only from the text file).

Once uploaded, the prompts are available to modify in the field labeled "List of prompt inputs"

1. under the **Script** menu at the bottom of the page, select **Prompts from file or textbox**.

2. enter one prompt per line. It's easiest to put each parameter on a separate lines and delete the `return` characters when you're done

![](images/divider.jpg)

[list of possible parameters](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/248):

- sd_model
- outpath_samples
- outpath_grids
- prompt_for_display
- prompt
- negative_prompt
- styles
- seed `integer`
- subseed_strength `floating point`
- subseed `integer`
- seed_resize_from_h `integer`
- seed_resize_from_w `integer`
- sampler_index `integer`
- sampler_name
- batch_size `integer`
- n_iter `integer`
- steps `integer`
- cfg_scale `floating point`
- width `integer`
- height `integer`
- restore_faces `boolean`
- tiling `boolean`
- do_not_save_samples `boolean`
- do_not_save_grid `boolean`

*`string` where no type is specified*

![](images/divider.jpg)

### Links

- https://www.reddit.com/r/StableDiffusion/comments/y2ohu9/guide_for_prompts_from_file_automatic1111/
- https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/7740
- https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/248
- https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/2332

