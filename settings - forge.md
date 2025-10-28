
<!-- vim: set foldmethod=marker fmr=###,--- :-->

### Forge Settings

- these settings are stored in `config.json`
- per-image defaults are stored in `ui-config.json`

----
#### saving images

saving images/grids: change format to jpg
change JPG quality to 95%
change width/height limit to 5000
check "save incomplete images" (at bottom)

----
#### stable diffusion

sampler parameters: hide all samplers but DPM++ 3M SDE
stable diffusion: check "enable comments" (already checked)
vae: fixFP16ErrorsSDXLLowerMemoryUse_v10.safetensors

----
#### user interface

live previews: format jpg, uncheck "Show previews of all images generated in a batch as a grid"
live previews: display period 2, method TAESD, update period 250ms
live previews: check "return image with chosen...", 
live previews: check "Show Live preview in full page image viewer"
prompt editing: uncheck "Alt+left/right moves prompt elements"
settings in UI: emphasis, notif. volume
NOT UI alternatives: hires fix: show checkpoint & sampler
UI alternatives: hires fix: show prompt
UI alternatives: uncheck "don't interrupt"
user interface: sd_model_checkpoint, do not show images, live previews enable

----
#### system

system: "disable" Automatically open webui in browser on startup

----
#### postprocessing

postprocessing: enable postprocessing: upscaling
upscaling: upscaler for img2img: 4xUltrasharp_4xUltrasharpV10

----
#### uncategorized

adetailer: max tabs 1
adetailer: max tabs 1
adetailer: sort bounding boxes by: position
adetailer: try to match inpainting size to bounding box size: free
dynamic prompts: String to use as wrap for parser wildcard, .e.g __wildcard__

----
#### Defaults

save defaults (stored in ui-config.json)





