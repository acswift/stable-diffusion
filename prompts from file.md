<!-- ![](images/shadow.jpg)  -->
<!-- ![](images/divider.jpg) -->

*Updated 31 March, 2025 · Toulouse*

![](images/divider.jpg)

### Prompt from a File

This (for me) turned out to be a dead end:

- the file has to be uploaded (I was hoping to use a file on disk that is read each time while SD continues to operate in the background)
- it does not seem to be able to handle ADetailer or upscaling.

![](images/divider.jpg)

The prompts in the file will be added at the **start** or **end** of the prompts entered in the main prompt text field.

Empty the main prompt fields if you want prompts to come only from the text file.

Once uploaded, the prompts are available to modify in the field labeled `List of prompt inputs`.

1. under the **Script** menu at the bottom of the page, select **Prompts from file or textbox**.

2. enter one prompt per line. It's easiest to put each parameter on a separate lines and delete the `return` characters when you're done

![](images/divider.jpg)

The script `prompts_from_file.py` is in `stable-diffusion-webui/scripts`.

It sort of looks like you could just add in the parameters that are not missing. Here's the data accompanying a sci-fi image:
```
parameters
a rocky dusty landscape on an alien planet
Negative prompt: boring, plain
Steps: 30, Sampler: DPM++ 3M SDE, Schedule type: Karras, CFG scale: 5, Seed: 2928875613, Size: 1024x768, Model hash: ff5b69c9a1, Model: zavychromaxl_v100, VAE hash: 235745af8d, VAE: fixFP16ErrorsSDXLLowerMemoryUse_v10.safetensors, Version: v1.10.1
```

<details><summary>file contents</summary>

```
import copy
import random
import shlex

import modules.scripts as scripts
import gradio as gr

from modules import sd_samplers, errors, sd_models
from modules.processing import Processed, process_images
from modules.shared import state


def process_model_tag(tag):
    info = sd_models.get_closet_checkpoint_match(tag)
    assert info is not None, f'Unknown checkpoint: {tag}'
    return info.name


def process_string_tag(tag):
    return tag


def process_int_tag(tag):
    return int(tag)


def process_float_tag(tag):
    return float(tag)


def process_boolean_tag(tag):
    return True if (tag == "true") else False


prompt_tags = {
    "sd_model": process_model_tag,
    "outpath_samples": process_string_tag,
    "outpath_grids": process_string_tag,
    "prompt_for_display": process_string_tag,
    "prompt": process_string_tag,
    "negative_prompt": process_string_tag,
    "styles": process_string_tag,
    "seed": process_int_tag,
    "subseed_strength": process_float_tag,
    "subseed": process_int_tag,
    "seed_resize_from_h": process_int_tag,
    "seed_resize_from_w": process_int_tag,
    "sampler_index": process_int_tag,
    "sampler_name": process_string_tag,
    "batch_size": process_int_tag,
    "n_iter": process_int_tag,
    "steps": process_int_tag,
    "cfg_scale": process_float_tag,
    "width": process_int_tag,
    "height": process_int_tag,
    "restore_faces": process_boolean_tag,
    "tiling": process_boolean_tag,
    "do_not_save_samples": process_boolean_tag,
    "do_not_save_grid": process_boolean_tag
}


def cmdargs(line):
    args = shlex.split(line)
    pos = 0
    res = {}

    while pos < len(args):
        arg = args[pos]

        assert arg.startswith("--"), f'must start with "--": {arg}'
        assert pos+1 < len(args), f'missing argument for command line option {arg}'

        tag = arg[2:]

        if tag == "prompt" or tag == "negative_prompt":
            pos += 1
            prompt = args[pos]
            pos += 1
            while pos < len(args) and not args[pos].startswith("--"):
                prompt += " "
                prompt += args[pos]
                pos += 1
            res[tag] = prompt
            continue


        func = prompt_tags.get(tag, None)
        assert func, f'unknown commandline option: {arg}'

        val = args[pos+1]
        if tag == "sampler_name":
            val = sd_samplers.samplers_map.get(val.lower(), None)

        res[tag] = func(val)

        pos += 2

    return res


def load_prompt_file(file):
    if file is None:
        return None, gr.update(), gr.update(lines=7)
    else:
        lines = [x.strip() for x in file.decode('utf8', errors='ignore').split("\n")]
        return None, "\n".join(lines), gr.update(lines=7)


class Script(scripts.Script):
    def title(self):
        return "Prompts from file or textbox"

    def ui(self, is_img2img):
        checkbox_iterate = gr.Checkbox(label="Iterate seed every line", value=False, elem_id=self.elem_id("checkbox_iterate"))
        checkbox_iterate_batch = gr.Checkbox(label="Use same random seed for all lines", value=False, elem_id=self.elem_id("checkbox_iterate_batch"))
        prompt_position = gr.Radio(["start", "end"], label="Insert prompts at the", elem_id=self.elem_id("prompt_position"), value="start")

        prompt_txt = gr.Textbox(label="List of prompt inputs", lines=1, elem_id=self.elem_id("prompt_txt"))
        file = gr.File(label="Upload prompt inputs", type='binary', elem_id=self.elem_id("file"))

        file.change(fn=load_prompt_file, inputs=[file], outputs=[file, prompt_txt, prompt_txt], show_progress=False)

        # We start at one line. When the text changes, we jump to seven lines, or two lines if no \n.
        # We don't shrink back to 1, because that causes the control to ignore [enter], and it may
        # be unclear to the user that shift-enter is needed.
        prompt_txt.change(lambda tb: gr.update(lines=7) if ("\n" in tb) else gr.update(lines=2), inputs=[prompt_txt], outputs=[prompt_txt], show_progress=False)
        return [checkbox_iterate, checkbox_iterate_batch, prompt_position, prompt_txt]

    def run(self, p, checkbox_iterate, checkbox_iterate_batch, prompt_position, prompt_txt: str):
        lines = [x for x in (x.strip() for x in prompt_txt.splitlines()) if x]

        p.do_not_save_grid = True

        job_count = 0
        jobs = []

        for line in lines:
            if "--" in line:
                try:
                    args = cmdargs(line)
                except Exception:
                    errors.report(f"Error parsing line {line} as commandline", exc_info=True)
                    args = {"prompt": line}
            else:
                args = {"prompt": line}

            job_count += args.get("n_iter", p.n_iter)

            jobs.append(args)

        print(f"Will process {len(lines)} lines in {job_count} jobs.")
        if (checkbox_iterate or checkbox_iterate_batch) and p.seed == -1:
            p.seed = int(random.randrange(4294967294))

        state.job_count = job_count

        images = []
        all_prompts = []
        infotexts = []
        for args in jobs:
            state.job = f"{state.job_no + 1} out of {state.job_count}"

            copy_p = copy.copy(p)
            for k, v in args.items():
                if k == "sd_model":
                    copy_p.override_settings['sd_model_checkpoint'] = v
                else:
                    setattr(copy_p, k, v)

            if args.get("prompt") and p.prompt:
                if prompt_position == "start":
                    copy_p.prompt = args.get("prompt") + " " + p.prompt
                else:
                    copy_p.prompt = p.prompt + " " + args.get("prompt")

            if args.get("negative_prompt") and p.negative_prompt:
                if prompt_position == "start":
                    copy_p.negative_prompt = args.get("negative_prompt") + " " + p.negative_prompt
                else:
                    copy_p.negative_prompt = p.negative_prompt + " " + args.get("negative_prompt")

            proc = process_images(copy_p)
            images += proc.images

            if checkbox_iterate:
                p.seed = p.seed + (p.batch_size * p.n_iter)
            all_prompts += proc.all_prompts
            infotexts += proc.infotexts

        return Processed(p, images, p.seed, "", all_prompts=all_prompts, infotexts=infotexts)
```

</details>

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

