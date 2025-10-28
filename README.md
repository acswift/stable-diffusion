
<!-- vim: set foldmethod=marker fmr=###,--- :-->

*Updated 27 October, 2025 · Toulouse*

### Stable Diffusion WebUI Forge on Apple Silicon

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

> These instructions use bash (macOS uses the ZSH shell by default)

```
chsh -s /bin/bash # change to bash before continuing
```
```
chsh -s /bin/zsh # change back to ZSH when done
```
These instructions assume that you are familiar with Git and Github.

* * *

We will install Forge in the user's applications folder.

```
mkdir -p ~/Applications && cd ~/Applications
git clone git@github.com:lllyasviel/stable-diffusion-webui-forge.git
```
Check if `brew` is installed:
``` 
brew --version
```
If necessary, go to [brew.sh](https://brew.sh/), copy the prompt and paste it into Terminal.

* * *

Install ASDF:
```
brew install asdf
```
Add ASDF to the user's profile and restart the session:

# integrate into following command:

```
. "$HOME/.asdf/asdf.sh"
. "$HOME/.asdf/completions/asdf.bash"

export PATH="$HOME/.asdf/shims:$HOME/.asdf/bin:$PATH"
```
```
{ 
  echo -e ". $(brew --prefix asdf)/libexec/asdf.sh"
  cat ~/.profile
} > ~/.profile.tmp && mv ~/.profile.tmp ~/.profile
source ~/.profile
asdf version
```
Make sure Python plugin is installed:
```
asdf plugin add python
```
```
cd ~/Applications/stable-diffusion-webui-forge

asdf install python 3.10.14     # install Python 3.10.14, takes a long time
asdf local python 3.10.14       # just your Forge project
python --version                # check
```
---

* * *

**fix runtime errors**

```
cd ~/Applications/stable-diffusion-webui-forge

# 1. svglib install failure (pycairo build error)
brew install pkg-config cairo cmake

source venv/bin/activate
pip install svglib
pip install joblib
```

**fix errors two**
```
brew install xz

asdf uninstall python 3.10.14
export LDFLAGS="-L/opt/homebrew/opt/xz/lib"
export CPPFLAGS="-I/opt/homebrew/opt/xz/include"
export PKG_CONFIG_PATH="/opt/homebrew/opt/xz/lib/pkgconfig"
asdf install python 3.10.14

cd ~/Applications/stable-diffusion-webui-forge
rm -rf venv
python -m venv venv
source venv/bin/activate
pip install --upgrade pip
./webui.sh
```

</details><details><summary>Symlinks</summary>

### Symlinks

The point is to keep anything that's added to the program folder out of the program folder.

This way, it's possible to share models etc., between different installations.

**Configuration**

The following files are stored in this repo, to make configuration easier:
- `config.json` - settings
- `ui-config.json` - image-specific settings (saved from Other › Defaults)
- `user.css` - any user-created CSS

```
src="/Volumes/External/Repositories/stable-diffusion"
dest="/Users/Main/Applications/stable-diffusion-webui-forge"
```
```
rm -rf                                       "$dest/user.css"
ln -s "$src/aliases - forge/user.css"        "$dest/user.css"
rm -rf                                       "$dest/config.json" 
ln -s "$src/aliases - forge/config.json"     "$dest/config.json" 
rm -rf                                       "$dest/ui-config.json" 
ln -s "$src/aliases - forge/ui-config.json"  "$dest/ui-config.json" 
```
**Models & Extensions**

```
src="/Volumes/External/Stable Diffusion"
dest="/Users/Main/Applications/stable-diffusion-webui-forge"
```
```
rm -rf                          "$dest/models"
ln -s "$src/models"             "$dest/models"
rm -rf                          "$dest/embeddings"
ln -s "$src/models/embeddings"  "$dest/embeddings"
rm -rf                          "$dest/outputs"
ln -s "$src/outputs"            "$dest/outputs"
rm -rf                          "$dest/extensions"
ln -s "$src/extensions"         "$dest/extensions"
```
**Notification Sound**
```
rm -rf                                            "$dest/notification.mp3"
ln -s "$src/notification sounds/notification.mp3" "$dest/notification.mp3"
```
---

</details><details><summary>First Run</summary>

### First Run
```
cd ~/Applications/stable-diffusion-webui && ./webui.sh
```

---

</details><details><summary>Extensions</summary>

### Extensions

Install from URL then reload UI:
- https://github.com/Bing-su/adetailer.git
- https://github.com/adieyal/sd-dynamic-prompts.git

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


**Microsoft edge**

- system & performance › performance
- "never put these sites to sleep" —› add ip address
