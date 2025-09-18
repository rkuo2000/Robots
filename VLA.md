
## Vision-Language-Action Models (VLA)

**Arxiv**: [Vision-Language-Action Models: Concepts, Progress, Applications and Challenges](https://arxiv.org/html/2505.04769v1)<br>
<p><img width="50%" height="50%" src="https://arxiv.org/html/2505.04769v1/x1.png"></p>

<p><img width="50%" height="50%" src="https://arxiv.org/html/2505.04769v1/x3.png"></p>
<p><img width="50%" height="50%" src="https://arxiv.org/html/2505.04769v1/x4.png"></p>

---
### [OpenVLA](https://github.com/openvla/openvla)

```
# Install minimal dependencies (`torch`, `transformers`, `timm`, `tokenizers`, ...)
# > pip install -r https://raw.githubusercontent.com/openvla/openvla/main/requirements-min.txt
from transformers import AutoModelForVision2Seq, AutoProcessor
from PIL import Image

import torch

# Load Processor & VLA
processor = AutoProcessor.from_pretrained("openvla/openvla-7b", trust_remote_code=True)
vla = AutoModelForVision2Seq.from_pretrained(
    "openvla/openvla-7b", 
    attn_implementation="flash_attention_2",  # [Optional] Requires `flash_attn`
    torch_dtype=torch.bfloat16, 
    low_cpu_mem_usage=True, 
    trust_remote_code=True
).to("cuda:0")

# Grab image input & format prompt
image: Image.Image = get_from_camera(...)
prompt = "In: What action should the robot take to {<INSTRUCTION>}?\nOut:"

# Predict Action (7-DoF; un-normalize for BridgeData V2)
inputs = processor(prompt, image).to("cuda:0", dtype=torch.bfloat16)
action = vla.predict_action(**inputs, unnorm_key="bridge_orig", do_sample=False)

# Execute...
robot.act(action, ...)
```

---
### [Open X-Embodiment](https://github.com/google-deepmind/open_x_embodiment)
#### Arxiv: [Open X-Embodiment: Robotic Learning Datasets and RT-X Models](https://arxiv.org/html/2310.08864v9)<br>

<p><img width="50%" height="50%" src="https://github.com/google-deepmind/open_x_embodiment/raw/main/imgs/teaser.png"></p>

---
### [Robotics Transformer (RT-1)](https://research.google/blog/rt-1-robotics-transformer-for-real-world-control-at-scale/)
![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEj11ho9tm4Td7ByTigAgDxFWsxbsZ6tQsAng3AtwuufHRuoLaLOV9YN7FUMyyAhefzuFOVCrbwTLsEaRYidOOToS__KRrotot-6aBxTliZxYz-B2jiJG-4myq5NB3vRKaY86nr5y1-13dBv_H_XyfnDijphCM3UBalczim0PeGJ63Z0Ok6k9zvKQ2D55A/s16000/image1.png)


---
### [BridgeData V2](https://rail-berkeley.github.io/bridgedata/)

#### Arxiv: [BridgeData V2: A Dataset for Robot Learning at Scale](https://arxiv.org/html/2308.12952v3)

#### Github: [https://github.com/rail-berkeley/bridge_data_v2](https://github.com/rail-berkeley/bridge_data_v2)

---
