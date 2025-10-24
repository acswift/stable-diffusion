
<!-- vim: set foldmethod=marker fmr=###,--- :-->

### controlnet

[Andy Hu's tutorial](https://andyhtu.com/how-to-install-controlnet-automatic1111-a-comprehensive-guide/)

> Image can be repared by changing `https://goldeyes.net/wpmedia/andyhtu/` to `https://andyhtu.com/wp-content/`

---

### Step 1: Installing ControlNet

One step is using the Automatic1111 Web UI, and the other step is using GitHub Desktop. Follow the method that best suits you.

![Load ControlNet from Extension](images/Load-ControlNet-From-Extension.jpg)

![Find and Install SD WebUI ControlNet Manipulation Extension](images/find-and-install-sd-webui-controlnet-manipulation-extension.jpg)


**Option 1 – Using the Automatic1111 Web UI:**

1. Open up your Automatic1111 Web UI.
2. Go to the Extensions Tab > Available tab > click “Load From.”
3. In the search bar,
4. Type in “controlnet.” You’ll see “sd_webui_controlnet” listed below.
5. Click on “Install” to the right side.
6. Go to the “Installed” tab > Apply and restart UI.

* * *

**Option 2 – For GitHub Desktop Users:**

This method is my preferred way to install, as it’s cleaner and helps tidy things up. You can always uninstall it using this method in case something goes wrong.

Find the Link:

1. Open up your Automatic1111 Web UI  
   - Go to “Extensions” > “Available” > Click “Load From” Search “control” > right-click “sd_webui_controlnet” > copy link Address.
   - The link will be: ” https://github.com/Mikubill/sd-webui-controlnet”
2. In Git:
```
cd ~/Applications/stable-diffusion-webui/extensions
git clone git@github.com:Mikubill/sd-webui-controlnet.git
```
3. Open up Automatic1111 WebUI.

Now you should see a ControlNet option in the main screen under the txt2img and img2img tab. It will also appear on other tabs if you have them installed for other extensions.

---
### Step 2: Installing the Models (51 GB)

You’ll notice that the Preprocessor and Model drop-down menu doesn’t show anything in the models folder. That’s because you’ll now need to install the models for ControlNet.

1. Download the Files:
   - https://huggingface.co/lllyasviel/ControlNet-v1-1/tree/main (28 items)
   - https://huggingface.co/lllyasviel/sd_control_collection/tree/main (SDXL) (43 items)
2. You will need both the .yaml file (configuration) and the .pth file (larger, indicated by the “LFS” logo).
   - Click on the down arrow to download all these files.
   - (You don’t need all of them, just the ones you want, but I’d download all to keep the instructions simple.)
3. Drop all files in the Stable Diffusion Automatic1111 installation folder:
   - Extensions > sd-webui-controlnet > models > 
   - drop all the control_V11 .pth and .yml files.
4. Open A1111:
   - All the ControlNet models should now populate under the drop down menu.

You’re now done with Installing ControlNet for Automatic1111.

![ControlNet Unit Panel](images/ControlNet-Control-panel.jpg)

---
### Post-Installation Tips and Best Practices

Using ControlNet with an OpenPose model can be a bit tricky, especially if you’re new to the process. Here’s a consolidated guide to help you get the best results:

Understanding Preprocessors: Preprocessors build a model image from a standard image to be used by the corresponding model in ControlNet. If you’re using an image already compatible with OpenPose (like a “stick person” template), there’s no need to use a preprocessor.

In other words:
- If your input is a regular photo of a person, use both the preprocessor and model set to OpenPose.
- If your input is already an OpenPose image, set the preprocessor to “none” and the model to OpenPose.

Dealing with Preprocessed Images: If OpenPose isn’t working, it might be because your input image is already preprocessed. ControlNet is highly sensitive to the data it receives. Even a minor variation in pixel color value can disrupt the image.
- Ensure that the preprocessor field is empty for an already synthesized picture to allow OpenPose to work.
- If it still doesn’t function, your image might be at fault.  
  In that case, load a fresh image and extract the pose or import an already optimized picture obtained through programs explicitly compatible with ControlNet (like the OpenPose loader extension for Automatic1111).

Remember, ControlNet is very particular about the images it processes. Always consider whether your image requires preprocessing or if it’s already optimized for use with OpenPose. Follow these guidelines, and you’ll find the process smoother and more intuitive. Have fun!

ControlNet References: [Mikubill / SD-Webui-ControlNet](https://github.com/Mikubill/sd-webui-controlnet/blob/main/README.md)

---
### The ControlNet Blueprint

- [Why ControlNet is Setting New Standards in AI Art Generation](https://www.andyhtu.com/post/what-is-controlnet-ai-art)
- [How to Install ControlNet Automatic1111: A Comprehensive Guide](https://www.andyhtu.com/post/how-to-install-controlnet-automatic-1111-a-comprehensive-guide)
- [Understanding ControlNet Interface in Automatic1111 Web UI](https://www.andyhtu.com/post/Understanding-ControlNet-Interface-in-Automatic-1111-Web-UI)

---
### errors

I got the following errors because my Python was out of date.

- Error loading script: api.py
- Error loading script: batch_hijack.py
- Error loading script: controlnet.py
- Error loading script: external_code.py
- Error loading script: infotext.py
- Error loading script: xyz_grid_support.py

The fix
```
brew install python@3.10
# Python is installed as
# /opt/homebrew/bin/python3.10
```
```
cd /Users/Main/Applications/stable-diffusion-webui
rm -rf venv
python3.10 -m venv venv
source venv/bin/activate
```
- Restart A1111 (it restarted without errors)

Well actually, one error:
- insightface warning: You can fix it by running:
```
cd ~/Applications/stable-diffusion-webui/
source venv/bin/activate
pip install insightface
```
This created a new problem:
> insightface pulled in numpy 2.x and protobuf 6.x, but A1111 and its dependencies (like gradio, mediapipe, open-clip-torch, etc.) require numpy < 2 and protobuf < 4.

Fix:
```
source venv/bin/activate
pip install "numpy<2" "protobuf<4" --force-reinstall
pip install insightface --no-deps
```

---
### blurry images

Edit your launch command to enable MPS (Apple GPU) instead of forcing CPU.

1. Open webui-user.sh in your Stable Diffusion folder.
```
cd /Users/Main/Applications/stable-diffusion-webui
source venv/bin/activate
python -m pip install --upgrade torch torchvision torchaudio
```
```
python -c "import torch; print(torch.backends.mps.is_available())"
# should print True
```
Start A111 using:
```
./webui.sh --no-half-vae
```
Search output for "Using device: mps"


Remove --use-cpu interrogate and --skip-torch-cuda-test.

Add this instead:

export PYTORCH_ENABLE_MPS_FALLBACK=1
./webui.sh --no-half-vae

--- 

### what worked

The error was because after upgrading Python, the PyTorch + torchvision versions you tried are incompatible. On macOS / Apple Silicon, the correct combo for SDXL + MPS is:

- torch 2.3.0
- torchvision 0.18.0
- torchaudio 2.3.0

PyTorch provides macOS arm64 wheels only for certain version combos. Try this:
```
source venv/bin/activate
pip uninstall torch torchvision torchaudio -y
pip install torch==2.3.0 torchvision==0.18.0 torchaudio==2.3.0 --index-url https://download.pytorch.org/whl/cpu
```
update `webui-macos-env.sh`:
```
export COMMANDLINE_ARGS="--skip-torch-cuda-test --upcast-sampling"
```
---

<details><summary>other</summary><br>

#### other

https://github.com/Mikubill/sd-webui-controlnet.git

cd ~/stable ... extensions/
git clone https://github.com/Mikubill/sd-webui-controlnet.git

there was  lot of downloading, but it looks like I chould have cloned it
https://huggingface.co/docs/hub/en/repositories-getting-started

https://github.com/lllyasviel/ControlNet/issues/149
try this
delete the Controlnet folder from your extensions. then after restarting Ui you have to do 2steps:
1.install it from url: paste https://github.com/lllyasviel/ControlNet.git
2.install it from available extensions. look for controlnet and install the one with 1200steps or more!


"st webui controlnet maniupulations" with 17000+ start





https://github.com/lllyasviel/ControlNet

install from URL


https://www.reddit.com/r/StableDiffusion/comments/119o71b/a1111_controlnet_extension_explained_like_youre_5/



downloaded all from https://huggingface.co/webui/ControlNet-modules-safetensors/tree/main
moved to extensions/contorlnet/models
restarted UI




https://www.reddit.com/r/StableDiffusion/comments/12na7ic/controlnet11_arrived_in_a1111_extension/


https://huggingface.co/lllyasviel/ControlNet-v1-1/tree/main



https://www.reddit.com/r/StableDiffusion/comments/11cwiv7/collected_notes_and_observations_on_controlnet/

---

</details>
