## new installation 250523 after reForge didn't work out

## `brew install cmake protobuf rust python@3.10 git wget`

## `cd && git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui`

## new installation 230314 after memory errors

## `https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Installation-on-Apple-Silicon`

## `brew install cmake protobuf rust python@3.10 git wget` `cd && git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui`  `# view all sizes of main dir to find all models, loras and embeddings` `# move MP3's and user.css` `# run stable diffusion`

copy to extensions folder:

* install prompt extensions  
* install adetalier

original source:

* https://github.com/Bing-su/adetailer.git  
* https://github.com/adieyal/sd-dynamic-prompts.git

still got out of memory error

* need to check system logs  
* launched by adding `~/stable-diffusion-webui/webui.sh --medvram`  
  `/opt/homebrew/Cellar/python@3.10/3.10.16/Frameworks/Python.framework/Versions/3.10/Resources/Python.app/Contents/MacOS/Python`  
* we'll see what happens  
  try in safari, may be that Edge is the problem

## promp swapping

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Features\#prompt-editing

* `[prompt1:prompt2:whentoswitch]`  
  `whentoswitch = n° or steps or 0-1 percent`  
* `[addedprompt:whentoadd]`  
* `[removedprompt::whentoremove]`

\[from:to:when\]

* from and to are arbitrary texts  
* when is a number that defines when the switch happens  
  if when is a number from 0.0 to 1.0, it's a fraction of the number of steps  
  if it's an integer, it's the step after which to make the switch

Additionally:

* \[to:when\] \- adds to to the prompt after a fixed number of steps (when)  
* \[from::when\] \- removes from from the prompt after a fixed number of steps (when)

Alternating Words:

* \[cow|horse\] in a field  
  swapping every other step

## inpainting

###### detection

**detection model confidence threshold**: minimum confidence score needed to consider that it's really a face.

**mask max area ratio (0-1)**: if you set the min area ratio to 0.1, the extension will reject the detections with masks smaller than 10% of the size of the image

###### mask preprocessing

**mask x/y offset**: Move the mask in the x/y direction, in pixels.

**mask erosion/dilation** \= how much of original image is covered

**mask Merge mode**:

* None: Inpaint each mask.  
* Merge: Merge the masks and then inpaint.  
* Merge and invert: Inpaint the unmasked area.

###### inpainting 

**inpainting mask blur** affects the transition between pixels inside and outside the bounding box. A value set to zero can create harsh seams, while a high blur value can prevent the face from being detected for processing.

**inpaint denoising strength** 0-1 ratio of inpainting vs original

**inpaint only masked** You almost always want to use Inpaint only masked for inpainting faces.

**inpaint only mask padding** \= zoom

## settings

shift-return in prompt box to start new generation (if interrupt button not showing)

user interface › gallery		Do not show any images in gallery	  
user interface › live previews	Show live previews of the created image  
					Show Live preview in full page image viewer	

saving images/grids			File format for images —› jpg  
saving images/grids			UNCHECK Always save all generated image grids  
saving images/grids			Always save all generated image grids —› jpg

user interface › prompt editing	UNCHECK Alt+left/right moves prompt elements	  
user interface › settings in UI (get names by changing a setting, then seeing the alert when applied):

* sd\_vae  
* CLIP\_stop\_at\_last\_layers  
* face\_restoration  
* use\_downcasted\_alpha\_bar  
* pad\_cond\_uncond  
* hypertile\_enable\_unet  
* hypertile\_max\_tile\_unet  
* token\_merging\_ratio  
* token\_merging\_ratio\_hr

stable diffusion › stable diffusion	Upcast cross attention layer to float32	saved 45 seconds

system › system			Automatically open webui in browser on startup	

install ADetailer in extras, I forget the exact way	

---

## checkpoint directory

If you already downloaded one or more Stable Diffusion models, and you are using them across multiple user interfaces (A1111, InvokeAI, etc), you might NOT want to copy the .ckpt files in stable-diffusion-webui/models/Stable-diffusion to avoid duplications and unnecessary consumption of storage.

Instead, you can keep all your models in a separate folder and launch A1111 by adding the \--ckpt-dir flag.

For example, if you keep your models in /Users/YourUserName/Desktop/Models, then you can launch A1111 by writing in each of the next steps:  
./webui.sh \--ckpt-dir "/Users/YourUserName/Desktop/Models/"

Notice that your folder Models can have as many subfolders as you want to keep models organized. A1111 will parse all of them and list all .ckpt in the drop-down menu.

## noah goodhart

solution on [reddit](https://www.reddit.com/r/StableDiffusion/comments/1c0w01f/seeking_macbook_pro_automatic1111_optimized_setup/)  
512x512x15 steps, times were the same with and without \--no-half

could it be that it only affects inpainting? It wouldn't be so bad if it was the case.

1020x1024x30s:

* rien: 3m08  
* no-half: 3m23

tried the webu-user update:

## `export COMMANDLINE_ARGS="--skip-torch-cuda-test --upcast-sampling --opt-split-attention"`

1020x1024x30s:

* rien: 3m7.5

trying with

## `PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.0 ./webui.sh`

it shouldn't change anything because it only affects the limit, not the operation

next try with inpainting… still crashed

---

## honzajavorek on github

[this](https://github.com/AUTOMATIC1111/stable-diffusion-webui/issues/9133) worked for inpaiting:

honzajavorek commented on May 15, 2023 •   
Regarding the settings, you can put the environment variable to your webui-user.sh as well. This is how my look like right now:

## `#!/bin/bash`

## `#########################################################`

## `# Uncomment and change the variables below to your need:#`

## `#########################################################`

## 

## `# Install directory without trailing slash`

## `#install_dir="/home/$(whoami)"`

## 

## `# Name of the subdirectory`

## `#clone_dir="stable-diffusion-webui"`

## 

## `# PyTorch settings`

## `export PYTORCH_MPS_HIGH_WATERMARK_RATIO=0.7`

## `export PYTORCH_ENABLE_MPS_FALLBACK=1`

## 

## `# Commandline arguments for webui.py, for example: export COMMANDLINE_ARGS="--medvram --opt-split-attention"`

## `export COMMANDLINE_ARGS="--skip-torch-cuda-test --upcast-sampling --no-half-vae --no-half --opt-sub-quad-attention --use-cpu interrogate"`

## 

## `# python3 executable`

## `#python_cmd="python3"`

6m58 with inpainting, 3m40 without (worse than with \--no-half)

---

## automatic1111 github

https://github.com/AUTOMATIC1111/stable-diffusion-webui/wiki/Installation-on-Apple-Silicon

1. ./webui.sh \--opt-split-attention-v1  
   rien: 3m38 (30s slower than default)  
2. ./webui.sh \--opt-split-attention-v1 \--medvram  
   rien: 3m60  
3. ./webui.sh \--opt-split-attention-v1 \--lowvram  
   rien: 4m38  
4. .webui-user.sh: export COMMANDLINE\_ARGS="--skip-torch-cuda-test \--no-half \--use-cpu all"  
   rien: 13m03

---

## new instructions on github

https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/5461

found article about CoreML and SD

https://github.com/apple/ml-stable-diffusion

nothing useful

## random

./webui.sh \--no-half-vae

* rien 3m13

## next

https://huggingface.co/stabilityai/stable-diffusion-xl-base-1.0/discussions/25  
https://huggingface.co/docs/diffusers/v0.6.0/en/optimization/mps

check for updates, nightly builds are faster?

## forge

https://github.com/lllyasviel/stable-diffusion-webui-forge/issues/314

Just install it like A1111

* Create a new folder somewhere called "Forge" or something like this (not inside A1111 Folder, if you already have A1111).  
* Right click the folder and open new terminal tab  
* in Terminal type in: git clone https://github.com/lllyasviel/stable-diffusion-webui-forge and hit enter.  
* when done, close the terminal. There should be a new Folder (stable-diffusion-webui-forge) inside the Folder "Forge".  
* right click the "stable-diffusion-webui-forge" folder, open new terminal tab.  
* in Terminal type in: ./webui.sh and hit enter.  
* let Forge do the rest.

I would recommend to leave the A1111 installation untouched (if you already have A1111), so you can switch between base A1111 and Forge if needed. Whatever, this was the way i installed Forge on my Mac.

## inpainting 2

error in terminal: Try setting the "Upcast cross attention layer to float32" option in Settings \> Stable Diffusion

it's a checkbox, and I checked it — didn't help and I unchecked it

or using the \--no-half commandline argument to fix this. THIS WORKED

apparently it makes things much slower

---

better solution

https://www.reddit.com/r/StableDiffusion/comments/15yumjz/just\_say\_no\_to\_nohalf\_unless\_your\_gpu\_needs\_it/

The best solution to the 2.1 NaN problem is using one of the cross-attention optimizations, xformers, sdp, or sdp-no-mem. They eliminate the problem, and speed up, rather than slow down, image generation.

Sdp and sdp-no-mem can be enabled directly from Automatic1111's Optimizations\>Cross attention optimization setting. To use xformers, you need to have \--xformers in the commandline arguments in the webui-user.bat file:

set COMMANDLINE\_ARGS=--xformers

Having it there doesn't force you to use xformers as the optimization, it just allows you to. If the Cross attention optimization setting is either Automatic or xformers then xformers is used. Otherwise, whichever optimization that's selected is used. The other options include Sdp and Sdp-no-mem.

I tried to start with xformers but got "wheel" error (see https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/8188\#discussioncomment-7216546)

---

The pro tip move here is use photoshop/mspaint to make a crap drawing of a hat on the head then use img2img to make it "real".

https://stable-diffusion-art.com/adetailer/

---

## 241104 install

[https://github.com/viking1304/a1111-setup/discussions/2](https://github.com/viking1304/a1111-setup/discussions/2)

1. install homebrew: go to brew.sh online & copy installer command  
2. open a new terminal window and run  
   `brew install cmake protobuf rust python@3.10 git wget`  
3. type `cd` to be sure that you are in your home folder  
4. clone the web UI repository by running  
   `cd && git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui`  
5. place Stable Diffusion models/checkpoints you want to use into  
   `stable-diffusion-webui/models/Stable-diffusion`  
   (if you skip this step, the default model will be downloaded)

241209 did not do the following stuff

## Open webui-user.sh, find this line

`#export COMMANDLINE_ARGS=""`

and add the line below to use the recommended command line parameters for Mac

`export COMMANDLINE_ARGS="--skip-torch-cuda-test --opt-sub-quad-attention --upcast-sampling --no-half-vae --use-cpu interrogate"`

IN SD FOLDER

`curl "https://github.com/AUTOMATIC1111/stable-diffusion-webui/commit/ac4bfdb6434054a949384fe2b4b52e36e0be8db0.patch?full_index=1" | git apply -v --index`

Run `./webui.sh`

---

# older

https://github.com/AUTOMATIC1111/stable-diffusion-webui/discussions/7453

---

on intel: https://github.com/viking1304/a1111-setup

---

settings › stable diffusion › stable diffusion › √ Upcast cross attention layer to float32

shaved 45 seconds

---

## research

https://huggingface.co/docs/diffusers/en/optimization/mps  
https://pytorch.org/get-started/locally/

---

## versions

python3 \--version  
Python 3.12.2

python3 \-m pip \--version  
pip 24.0 from /opt/homebrew/lib/python3.12/site-packages/pip (python 3.12)

conda list anaconda$  
\-bash: conda: command not found

python3  
Python 3.12.2 (main, Feb  6 2024, 20:19:44) \[Clang 15.0.0 (clang-1500.1.0.2.5)\] on darwin  
Type "help", "copyright", "credits" or "license" for more information.  
\>\>\> import torch;  
Traceback (most recent call last):  
  File "\<stdin\>", line 1, in \<module\>  
ModuleNotFoundError: No module named 'torch'  
\>\>\> torch.\_\_version\_\_

---

https://www.reddit.com/r/StableDiffusion/comments/1c0w01f/seeking\_macbook\_pro\_automatic1111\_optimized\_setup/

UPDATE: Working MUCH faster now thanks to the below \#COMMANLINE\_ARGS="" adjustment to the /stable-diffusion-webui/webui-user.sh

export COMMANDLINE\_ARGS="--skip-torch-cuda-test \--upcast-sampling \--opt-split-attention"

Additionally launching with PYTORCH\_MPS\_HIGH\_WATERMARK\_RATIO=0.0 ./webui.sh 

AND Enabling Upcast cross attention layer to float32 under settings.

https://github.com/lllyasviel/Fooocus/discussions/1156

Edit the launch.py and add: os.environ\["PYTORCH\_MPS\_HIGH\_WATERMARK\_RATIO"\] \= "0.0"

it's a system variable, used before launching SD  
so I added it to my .bashrc

---

https://www.reddit.com/r/StableDiffusion/comments/183bxod/how\_to\_increase\_speed\_of\_autom1111\_on\_m1\_mac/

Also add these in webui-user.sh:

export COMMANDLINE\_ARGS="--opt-split-attention-v1 \--no-half \--skip-torch-cuda-test  
export PYTORCH\_MPS\_HIGH\_WATERMARK\_RATIO=0.0  
export PYTORCH\_ENABLE\_MPS\_FALLBACK=1

---

Use CoreML: No  
and this was \~68s. CoreML speeds up your render time, but increases the loading time (by a lot). If you switch models with any sort of frequency it's not worth it, but if you know you're going to use the same model for a while it might be worth turning on.

