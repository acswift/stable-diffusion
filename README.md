
<!-- vim: set foldmethod=marker fmr=###,--- :-->

*Updated 27 October, 2025 · Toulouse*

### Version Information

Ordered by last commit, oldest first:
- [Stable Diffusion web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) · Jul 27, 2024
- [Stable Diffusion WebUI Forge](https://github.com/lllyasviel/stable-diffusion-webui-forge) · Jun 26, 2025
- [Stable Diffusion WebUI Forge/reForge](https://github.com/Panchovix/stable-diffusion-webui-reForge) · Oct 25, 2025
- [Stable Diffusion WebUI Forge - Classic](https://github.com/Haoming02/sd-webui-forge-classic) · Oct 26, 2025
- [ersatzForge](https://github.com/DenOfEquity/ersatzForge) · Oct 26, 2025
- [Stable Diffusion WebUI Forge - Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo) · Oct 27, 2025


Forge Classic: https://github.com/Haoming02/sd-webui-forge-classic, from @Haoming02 with a lot of optimizations and features, from reforge, forge, etc based on old backend of forge.

Stable Diffusion WebUI Forge is a platform on top of the original Stable Diffusion WebUI by AUTOMATIC1111, to make development easier, optimize resource management, speed up inference, and study experimental features.

"Classic" mainly serves as an archive for the "previous" version of Forge, which was built on Gradio 3.41.2 before the major changes (see the original announcement) were introduced. Additionally, this fork is focused exclusively on SD1 and SDXL checkpoints, having various optimizations implemented, with the main goal of being the lightest WebUI without any bloatwares.

---

Stable Diffusion web UI

A web interface for Stable Diffusion, implemented using Gradio library.

No longer maintained.

---

Stable Diffusion WebUI Forge

Stable Diffusion WebUI Forge is a platform on top of Stable Diffusion WebUI (based on Gradio ) to make development easier, optimize resource management, speed up inference, and study experimental features.

The name "Forge" is inspired from "Minecraft Forge". This project is aimed at becoming SD WebUI's Forge.

Forge is currently based on SD-WebUI 1.10.1 at this commit. (Because original SD-WebUI is almost static now, Forge will sync with original WebUI every 90 days, or when important fixes.)

News are moved to this link: Click here to see the News section

---

Forge Neo: https://github.com/Haoming02/sd-webui-forge-classic/tree/neo, from @Haoming02. It is a continuation of Forge2 (so Flux, fp8, gguf, etc) but with more features (wan 2.2, Qwen Image, Nunchaku, etc), aimed on optimizations and new features.

"Neo" mainly serves as an continuation for the "latest" version of Forge, which was built on Gradio 4.40.0 before lllyasviel became too busy... Additionally, this fork is focused on optimization and usability, with the main goal of being the lightest WebUI without any bloatwares.

---

ersatzForge: https://github.com/DenOfEquity/ersatzForge, from DenOfEquity, based on Forge2, but as he says, with (experimental, opinionated) changes to Forge2 webUI.

a backup of my local (experimental, opinionated) changes to Forge2 webUI

---

### Stable Diffusion / A1111

Notes on how to get the best results from Stable Diffusion using Automatic1111 on an iMac M4.

---

<details><summary>Installation</summary>

### Installation
```
cd ~/Applications && git clone git@github.com:AUTOMATIC1111/stable-diffusion-webui.git
```
At this time, install:
- SD models (otherwise you'll have to wait while the default models are downloaded)
- notification sounds
- extensions if already downloaded
```
cd ~/Applications/stable-diffusion-webui && ./webui.sh
```
Check the Python version in the Terminal output — `3.10` is required for ControlNet.

---

</details><details><summary>Symlinks</summary>

### Symlinks

The following files are stored in this repo, to make configuration easier:
- `config.json` - settings
- `ui-config.json` - settings saved from the Other › Defaults settings tabs
- `user.css` - any user-created CSS

```
src="/Users/Main/Library/Mobile Documents/com~apple~CloudDocs/Repositories"
dest="/Users/Main/Applications"
```
```
rm -rf                                                      "$dest/stable-diffusion-webui/user.css"
ln -s "$src/stable-diffusion/aliased files/user.css"        "$dest/stable-diffusion-webui/user.css"
rm -rf                                                      "$dest/stable-diffusion-webui/config.json" 
ln -s "$src/stable-diffusion/aliased files/config.json"     "$dest/stable-diffusion-webui/config.json" 
rm -rf                                                      "$dest/stable-diffusion-webui/ui-config.json" 
ln -s "$src/stable-diffusion/aliased files/ui-config.json"  "$dest/stable-diffusion-webui/ui-config.json" 
```
---

</details><details><summary>Follow Up</summary>

### Follow Up

- [many custom scripts](https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Custom-Scripts#shift-attention)
- [a user script that adds a processing queue to the web ui](https://github.com/Kryptortio/SDAtom-WebUi-us)

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/features

https://www.aiarty.com/stable-diffusion-prompts/stable-diffusion-prompt-guide.htm

---
</details>
