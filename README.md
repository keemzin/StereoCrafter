# StereoCrafter WEBUI

A cloud-ready web interface for [StereoCrafter](https://github.com/TencentARC/StereoCrafter) by Tencent ARC Lab, built on top of [Enoky's](https://github.com/enoky/StereoCrafter) GUI implementation. Tested with a minimum RTX 6000 Ada (Lovelace architecture) and 48GB VRAM.

## Why a Web UI?

The original StereoCrafter uses CLI and Enoky's tkinter-based desktop GUI, which work great locally but have limitations:

- **Limited to local hardware** - You're stuck with the GPU you bought. You cannot increase the VRAM.
- **No remote access** - You can't process videos from anywhere.
- **Resource intensive** - High-end GPUs are expensive to own. Yet they output results faster.
- **OS, Batch and UI** - StereoCrafter is command-line only. Enoky is GUI-based with batch processing, but optimized for Windows 10/11 and may error on Linux/MacOS.

### Our Solution: Cloud-Ready Web Interface

By converting to a **Gradio-based web UI**, we enabled:

✅ **RunPod Integration** - Access high-end GPUs (RTX 4090, A6000) for pennies per hour  
✅ **Remote Processing** - Process videos from any device with a browser  
✅ **Cost Effective** - Pay only for GPU time you use, not ownership  
✅ **Scalability** - Spin up multiple instances for batch processing  
✅ **Accessibility** - Share processing power with team members  

**Example Cost Savings:**
- RTX 4090 ownership: ~$1,600 upfront
- RunPod RTX 4090: ~$0.50/hour (only when processing)
- Process 10 hours/month: $5 vs $1,600 investment

### Project Status

⚠️ **This is a passion project built in free time!**

This codebase is functional but not fully optimized. Built out of love for the technology — contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## 🚀 RunPod Setup

### 1. Select Pod Template
- Navigate to **Pod Template** → **All**
- Choose **Runpod Pytorch 2.8.0**

### 2. Configure Hardware
- Click **Configure Pod**
- Select GPU: **RTX 6000 Ada** (minimum — do not go below 48GB VRAM or non-Lovelace architecture)

### 3. Edit Container Settings

| Setting | Value |
| :--- | :--- |
| **Container Name** | `johnsdoes/stereocrafter-webui:rebase-v1.0.0` |
| **Container Disk** | `300 GB` *(Cost: ~$0.042/hr)* |
| **Volume Disk** | `0 GB` |
| **Expose HTTP Ports (StereoCrafter)** | `7860` |
| **Expose HTTP Ports (FileBrowser)** | `8080` |

### 4. Environment Variables
*   **Key:** `HF_TOKEN`
*   **Value:** `your huggingface read token`
*   **Key:** `FB_USERNAME`
*   **Value:** `your Filebrowsr username`
*   **Key:** `FB_PASSWORD`
*   **Value:** `your Filebrowser password`

### 5. Deploy
*   Click **Set Overrides**
*   Select **Deploy On-Demand**

---

## 🚀 Local Setup 

### Option 1: Docker (Recommended - Fastest & Easiest)

**Why Docker?**
- ✅ No dependency conflicts or DLL issues (especially on Windows)
- ✅ Simulates RunPod deployment environment exactly
- ✅ Pre-configured with all dependencies
- ✅ Works on Windows, Linux, and Mac
- ✅ One command to start everything

**Prerequisites:**
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed
- NVIDIA GPU with drivers installed
- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) (for GPU support)

**Quick Start:**

```bash
# Clone the repository
git clone https://github.com/keemzin/StereoCrafter.git
cd StereoCrafter

# Create .env file with your HuggingFace token
HF_TOKEN=your_huggingface_token_here
FB_USERNAME=admin
FB_PASSWORD=adminadmin12

# Start the container (downloads models on first run)
docker compose up -d

# View logs
docker compose logs -f

# Access:
StereoCrafter WebUI → http://localhost:7860
FileBrowser         → http://localhost:8080
```

**Docker Commands:**

```bash
docker compose down          # Stop
docker compose restart       # Restart after code changes
docker compose ps            # Status
docker compose exec stereocrafter-webui bash  # Shell access
docker compose down -v       # Remove everything including weights
```

---

### Option 2: Manual Setup (Python venv)

⚠️ **Windows users:** PyTorch DLL errors are common. Docker is strongly recommended.

**Prerequisites:**
- [Git](https://git-scm.com/downloads)
- [Python 3.12](https://www.python.org/downloads)
- [CUDA 12.8](https://developer.nvidia.com/cuda-12-8-0-download-archive)
- [FFmpeg](https://techtactician.com/how-to-install-ffmpeg-and-add-it-to-path-on-windows/)
- [HuggingFace Account](https://huggingface.co/join)

```bash
git clone https://github.com/keemzin/StereoCrafter.git
cd StereoCrafter

# Add upstream for syncing with enoky/StereoCrafter
git remote add upstream https://github.com/enoky/StereoCrafter.git
```

**Download model weights:**

```bash
mkdir weights && cd weights
git clone https://huggingface.co/TencentARC/StereoCrafter
git clone https://huggingface.co/tencent/DepthCrafter
git clone https://huggingface.co/stabilityai/stable-video-diffusion-img2vid-xt-1-1
cd ..
```

**Install and run:**

```bash
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Linux/Mac

pip install --upgrade pip
pip install -r requirements.txt
python webui.py
```

**Troubleshooting Windows DLL errors (`WinError 1114`):**

1. Install [VC++ 2015-2022 Redistributable (x64)](https://aka.ms/vs/17/release/vc_redist.x64.exe)
2. Verify CUDA: `nvidia-smi` and `nvcc --version`
3. Reinstall PyTorch:
   ```bash
   pip uninstall torch torchvision torchaudio
   pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
   ```
4. If still failing — use Docker.

---

## Honorable Mention

All credit for the core StereoCrafter technology goes to the original authors at **Tencent ARC Lab**. This project simply wraps their excellent work in a web interface for cloud deployment.

Original Paper: [StereoCrafter: Diffusion-based Generation of Long and High-fidelity Stereoscopic 3D from Monocular Videos](https://arxiv.org/abs/2409.07447)

## Citation

```bibtex
@article{zhao2024stereocrafter,
  title={Stereocrafter: Diffusion-based generation of long and high-fidelity stereoscopic 3d from monocular videos},
  author={Zhao, Sijie and Hu, Wenbo and Cun, Xiaodong and Zhang, Yong and Li, Xiaoyu and Kong, Zhe and Gao, Xiangjun and Niu, Muyao and Shan, Ying},
  journal={arXiv preprint arXiv:2409.07447},
  year={2024}
}
```

---

If you'd like to support this project, please consider registering for RunPod using my referral link:
https://runpod.io?ref=5r6ik1wp
