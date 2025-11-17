How to run Unmute on Runpod

<img width="1019" height="989" alt="Screenshot 2025-10-31 at 2 17 43 PM" src="https://github.com/user-attachments/assets/a1f5eea7-28d3-433b-8126-9b960353cb19" />

Here are the changes I needed to make to get it working on Runpod.

## Instructions 

Use this branch: https://github.com/tleyden/unmute/tree/runpod_frontend

### Configure Pod

Start a Runpod on-demand pod with:

1. Image: `runpod/pytorch:2.4.0-py3.11-cuda12.4.1-devel-ubuntu22.04`
2. L40s
3. 1 GPU
4. Container disk: 50 GB

Don't launch yet, there's one more thing needed.

### Setup port forwarding

Under **Expose HTTP Ports (Max 10)** setting in Runpod UI, set:

```
8888,3000,8000
```

(you can also remove the 8888 if you're not planning to use Jupyter notebooks)

Now launch the pod.

### Install dependencies

Once the pod is up, ssh in.

Runpod does not support docker-in-docker for GPU pods, so it has to be run dockerless.  Install the following:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
curl https://sh.rustup.rs -sSf | sh
curl -fsSL https://get.pnpm.io/install.sh | sh -
apt update && apt install -y pkg-config libssl-dev cmake build-essential
```

Logout and log back in.

### HuggingFace login

You need to login and accept the license for the model:

```
# uv pip install huggingface_hub
# huggingface-cli login
```

### Set env variables

Go to the Runpod UI under the **Connect** tab to get the forwarded URLs, which will look something like the ones below.

In the shell where you start the backend:

```
export UNMUTE_CORS_ALLOW_ORIGINS="https://your-instance-id-3000.proxy.runpod.net/"
```

and the front end:

```
export NEXT_PUBLIC_BACKEND_SERVER_URL="https://your-instance-id-8000.proxy.runpod.net/"
```

### Start services

See readme on how to start all dockerless services.

### Go to web page

Open https://your-instance-id-8000.proxy.runpod.net/ on your local browser
