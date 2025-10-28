
<!-- vim: set foldmethod=marker fmr=###,--- :-->

### Forge Settings · `config.json`

- per-image defaults are stored in `ui-config.json`

---
### saving images / saving images/grids

- File format for images: `jpg`
- Quality for saved jpeg and avif images `95%`
- Width/height limit for the above option, in pixels: `5000`
- Save incomplete images (at bottom): `√`

---
### stable diffusion

- sampler parameters, hide samplers: check all but `DPM++ 3M SDE`
- stable diffusion, emphasis mode: `No norm` 
- stable diffusion, enable comments: `√`
- vae, VAE type for encode: `TAESD` (being tested)

---
### user interface

**live previews**
- Live preview file format: `jpg`
- Show previews of all images generated in a batch as a grid: `uncheck`
- Live preview display period (in sampling steps: `2`
- Live preview method: `TAESD`
- Progressbar and preview update period: `250`
- Return image with chosen live preview: `√`
- Show Live preview in full page image viewer: `√`

**other:**
- prompt editing, Alt+left/right moves prompt elements: `uncheck`
- settings in UI: `emphasis`
- UI alternatives, Don't Interrupt in the middle: `uncheck`
- user interface, quicksettings list: `do_not_show_images`, `live_previews_enable`
- user interface, UI item order for txt2img/img2img tabs: `seed`

---
### system

- system: Automatically open webui in browser on startup: `Disable`

---
### postprocessing

- postprocessing, Enable postprocessing operations in txt2img and img2img tabs: `Upscale`
- upscaling, Upscaler for img2img: `4xUltrasharp_4xUltrasharpV10`

---
### uncategorized

- adetailer, max tabs: `1`
- adetailer, sort bounding boxes by: `Position (center to edge)`
- adetailer, try to match inpainting size to bounding box size: `Free`

---
### Defaults

- save defaults (stored in ui-config.json)

---
