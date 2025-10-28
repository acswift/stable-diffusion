
<!-- vim: set foldmethod=marker fmr=###,--- :-->

*Updated 27 October, 2025 · Toulouse*

### Stable Diffusion WebUI Forge

[Stable Diffusion WebUI Forge](https://github.com/lllyasviel/stable-diffusion-webui-forge) is the only stable and maintained version of the SD Web UI that is compatible with macOS.

---
<details><summary>SD Web UI Versions</summary>

#### Stable Diffusion web UI

- [Stable Diffusion web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) · Jul 27, 2024

A web interface for Stable Diffusion, implemented using Gradio library.

No longer maintained.

---

#### Stable Diffusion WebUI Forge

- [Stable Diffusion WebUI Forge](https://github.com/lllyasviel/stable-diffusion-webui-forge) · Jun 26, 2025

Stable Diffusion WebUI Forge is a platform on top of the original Stable Diffusion WebUI by AUTOMATIC1111, to make development easier, optimize resource management, speed up inference, and study experimental features.

Forge is currently based on SD-WebUI 1.10.1 at this commit. (Because original SD-WebUI is almost static now, Forge will sync with original WebUI every 90 days, or when important fixes.)

- [macOS instructions](https://github.com/lllyasviel/stable-diffusion-webui-forge/issues/2503)

---

#### Stable Diffusion WebUI Forge/reForge

- [Stable Diffusion WebUI Forge/reForge](https://github.com/Panchovix/stable-diffusion-webui-reForge) · Oct 25, 2025

The author specifically recommends using Classic or Neo for stability (implying that reForge will be unstable).

Stable Diffusion WebUI Forge/reForge is a platform on top of Stable Diffusion WebUI (based on Gradio) to make development easier, optimize resource management, speed up inference, and study experimental features.

Forge/reForge backend removes all WebUI's codes related to resource management and reworked everything. All previous CMD flags like medvram, lowvram, medvram-sdxl, precision full, no half, no half vae, attention_xxx, upcast unet, ... are all REMOVED. Adding these flags will not cause error but they will not do anything now.

---

#### Stable Diffusion WebUI Forge - Classic (no macOS support)

- [Stable Diffusion WebUI Forge - Classic](https://github.com/Haoming02/sd-webui-forge-classic) · Oct 26, 2025

Forge Classic: https://github.com/Haoming02/sd-webui-forge-classic, from @Haoming02 with a lot of optimizations and features, from reforge, forge, etc based on old backend of forge.

"Classic" mainly serves as an archive for the "previous" version of Forge, which was built on Gradio 3.41.2 before the major changes (see the original announcement) were introduced. Additionally, this fork is focused exclusively on SD1 and SDXL checkpoints, having various optimizations implemented, with the main goal of being the lightest WebUI without any bloatwares.

---

#### Stable Diffusion WebUI Forge - Neo (no macOS support)

- [Stable Diffusion WebUI Forge - Neo](https://github.com/Haoming02/sd-webui-forge-classic/tree/neo) · Oct 27, 2025

Forge Neo: https://github.com/Haoming02/sd-webui-forge-classic/tree/neo, from @Haoming02. It is a continuation of Forge2 (so Flux, fp8, gguf, etc) but with more features (wan 2.2, Qwen Image, Nunchaku, etc), aimed on optimizations and new features.

"Neo" mainly serves as an continuation for the "latest" version of Forge, which was built on Gradio 4.40.0 before lllyasviel became too busy... Additionally, this fork is focused on optimization and usability, with the main goal of being the lightest WebUI without any bloatwares.

---

#### ErsatzForge

- [ersatzForge](https://github.com/DenOfEquity/ersatzForge) · Oct 26, 2025

ersatzForge: https://github.com/DenOfEquity/ersatzForge, from DenOfEquity, based on Forge2, but as he says, with (experimental, opinionated) changes to Forge2 webUI.

a backup of my local (experimental, opinionated) changes to Forge2 webUI

---

### Stable Diffusion / A1111

Notes on how to get the best results from Stable Diffusion using Automatic1111 on an iMac M4.

---

</details><details><summary>Installation</summary>

### Installation

We will install Forge in the user's applications folder.

```
mkdir -p ~/Applications && cd ~/Applications
git clone git@github.com:lllyasviel/stable-diffusion-webui-forge.git
```
---

</details><details><summary>Symlinks</summary>

### Symlinks

After renaming the destination folders:
```
ln -s /Volumes/External/Stable\ Diffusion/models /Users/Main/Applications/stable-diffusion-webui/models
ln -s /Volumes/External/Stable\ Diffusion/models/embeddings /Users/Main/Applications/stable-diffusion-webui/embeddings 
ln -s /Volumes/External/Stable\ Diffusion/outputs /Users/Main/Applications/stable-diffusion-webui/outputs
```
The following files are stored in this repo, to make configuration easier:
- `config.json` - settings
- `ui-config.json` - settings saved from the Other › Defaults settings tabs
- `user.css` - any user-created CSS

```
src="/Volumes/External/Repositories"
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

</details><details><summary>Extensions</summary>

### Extensions

Install from URL then reload UI:
- https://github.com/Bing-su/adetailer.git
- https://github.com/adieyal/sd-dynamic-prompts.git

---

</details><details><summary>First Run</summary>

### First Run
```
cd ~/Applications/stable-diffusion-webui && ./webui.sh
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

---

At this time, install:
- notification sounds

**Microsoft edge**

- system & performance › performance
- "never put these sites to sleep" —› add ip address
