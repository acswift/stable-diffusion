
<!-- vim: set foldmethod=marker fmr=###,--- :-->

*Updated 25 October, 2025 · Toulouse*

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
src='/Users/Main/Library/Mobile Documents/com~apple~CloudDocs/Repositories'
dest='/Users/Main/Applications'
```
```
rm                                                                           "$dest/stable-diffusion-webui/config.json" 
ln -s "$src/stable-diffusion/aliased files/config.json"                      "$dest/stable-diffusion-webui/config.json" 
rm                                                                           "$dest/stable-diffusion-webui/ui-config.json" 
ln -s "$src/stable-diffusion/stable-diffusion/aliased files/ui-config.json"  "$dest/stable-diffusion-webui/ui-config.json" 
rm                                                                           "$dest/stable-diffusion-webui/user.css"
ln -s "$src/stable-diffusion/stable-diffusion/aliased files/user.css"        "$dest/stable-diffusion-webui/user.css"
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
