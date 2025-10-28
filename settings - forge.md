
<!-- vim: set foldmethod=marker fmr=###,--- :-->

### Forge Settings · `config.json`

- per-image defaults are stored in `ui-config.json`

----
#### saving images / saving images/grids

- File format for images: `jpg`
- Quality for saved jpeg and avif images `95%`
- Width/height limit for the above option, in pixels: `5000`
- Save incomplete images (at bottom): `√`

----
#### stable diffusion

- sampler parameters, hide samplers: check all but `DPM++ 3M SDE`
- stable diffusion, emphasis mode: `No norm` 
- stable diffusion, enable comments: `√`
- vaa, VAE type for encode: `TAESD` (being tested)

----
#### user interface

**live previews**
- Live preview file format: `jpg`
- Show previews of all images generated in a batch as a grid: `uncheck`
- Live preview display period (in sampling steps: `2`
- Live preview method: `TAESD`
- Progressbar and preview update period: `250`
- Return image with chosen live preview: `√`
- Show Live preview in full page image viewer: `√`

**other:**
- prompt editing: uncheck "Alt+left/right moves prompt elements"
- settings in UI: emphasis, notif. volume
- NOT UI alternatives: hires fix: show checkpoint & sampler
- UI alternatives: hires fix: show prompt
- UI alternatives: uncheck "don't interrupt"
- user interface: sd_model_checkpoint, do not show images, live previews enable

----
#### system

- system: "disable" Automatically open webui in browser on startup

----
#### postprocessing

- postprocessing: enable postprocessing: upscaling
- upscaling: upscaler for img2img: 4xUltrasharp_4xUltrasharpV10

----
#### uncategorized

- adetailer: max tabs 1
- adetailer: max tabs 1
- adetailer: sort bounding boxes by: position
- adetailer: try to match inpainting size to bounding box size: free
- dynamic prompts: String to use as wrap for parser wildcard, .e.g __wildcard__

----
#### Defaults

- save defaults (stored in ui-config.json)





